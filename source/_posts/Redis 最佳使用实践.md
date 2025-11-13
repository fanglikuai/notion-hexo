---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3Q2BTV3%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEfpmUrWYN2R59Erlecox7j0I%2BvnOnVB8J6cIc96a%2FOwAiEAp5yK9%2Bvi4s76CBgUCrGPUDRlJf68ZaQbmFBvdJLXqdwq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDHNMU5G10SUUzdDQgircA26KIPljov3DUnzubYqakhdcDqdf0ondzD%2FkYVswBA9pWt1Lh8W3PSZr6LqUbiOXmMFvoOVXnbB8iJaMrIckhMPcZlRHxrAXhXlaoWzJ%2FsA5cc0d2aswPc7ZvWPP0UfziNUdZE2E%2Fe7AUW9TAJxdCHK3siChVUvJ%2FKYWBAVymp32Izqz2ZjP3xt043uAZbNqe6m0jkkWLhNeozwlAkYJ81AdCiunWHIdzTiiu%2F%2BsBi6OwZLqMDoJ14dm8pJ5ZOI6cafZroCUKJT7%2BHqL4SoxZDAwFf%2B5pJT8Xa83IrAByjame9Y2s7YPdAf66RUdmInXUyW7LUfs22VPU0%2FLfvaDOnkTI%2BmYcEx4BwJFSA219snaShdDkLtmxiLwHPwlhv4uanE66CV16altqNYoIFcaEX4IT4JD3iBj8T8Ip9DXe9tV6AMw7UHuZWEFl5IEVshiz2Y4clDXRAyYSiM3ZVbuN6uw8fjCpf1aZBLco3xUytNz4e5BZkDcYH%2F%2FB9F871S8fecEbF4CQRm3elUFX6BqcZnszxrW8Ct1MqjV3Ko2AD61fx6FCrIMxAHpxzVkop9fBrXHmK%2BBCOacbP8V2V0UfdgHLO1ta0Lc%2BUSz8z1IApQxKRgrA9JberIg7qr9MJOy2cgGOqUBY9KQXkvNfz%2F4T54rYiJd6M9d0DSF4K%2BpeDjfjg2IPYwQftILAaOjDHL3SwsAA%2Bz0WNOx%2BPOpIt4oU5sjoUXZyb2fVraMBwpFFQNKmNRbPFeIvAiyuDE3JKcECEESq1RqUmd5LKcMHHbZpFquFl80D0sChA9YnhGfRFXS%2B44uqPK04A8%2FECPKptnJu9IiQ3H4vC9wb%2FI3GP%2F09gnRmOTg6yKLfnUg&X-Amz-Signature=2902f31459b50fd00d88194218b8c41dbd4cc0476b128bfeb0682fd812b2c9bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

