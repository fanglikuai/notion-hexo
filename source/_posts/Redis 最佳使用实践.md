---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD5WR6KY%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHHxy1PzUqS%2F4hBNvSpSdpUMR7N3USlT73ieAM%2FAD4uvAiEA465k3sNB6wxOaCSbn3l0TGpaPbei688FlDAZrJPyoTIq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDFBByUxTnZHdsHC5OCrcA0ltRfosXiNirPzSWDu%2B%2Ba8vCxQVe7LPfVko7OWw9uQSyXCfZdXvv%2BxeRuByStPlhfiuOAFaTvcDQqxDqc%2FRk1O%2FLs7HsZmpXV2%2Br1glopqpgfyTK6Sru%2B85Y%2FvGoIcXNc6g6fAO6%2B6S5EsElDEH7n7dn7KupcDmgo%2BAJqwiFtqnRllGwUK6EmHth70118yeODcVc0dYmlarw7qE7UVNhXiMkZnOhlFvQPgzfma5yhliv6hVg2L3iZC8F%2FxAg7s10oqWUdf6rWMtlxoV0rxozjZ4jaYr3pYT2OvUtr18Rh8K5iM9xsyVwBz8uAvCSgWK3BBRu0eo12kGikdt9aIKveMk7CAeXuPn9q6F2MiQ6rZTOrWfzwSQJtalh9ZpezneA3MbTfijs0OmZ%2BXmoEEFnkTUmYWKhZEV6VaJhmxyXcD2AYW%2BSq1ioeeyjPmvd%2BDcMBdJrmkfDMPc4BwRC02CmSaXVVPVL7HlGxE5TxVpByWEEkbIKLZmV13DA6tupjhq0QbaV7df1xqc4NDaFjQSWMdU5EU7eZ1x6oi0QRl7Y%2FRiTQcDJDY729AIRgPO%2Bt3Iwzq8tsymkztOWcMywzEFHtSpu6JcQ8swrq60eK0klC3oE39yQyfjZmidtz3fMMnQo8gGOqUBiUpSomUNnK1Z%2Fp7HzATSEUl9mK4JisKmgK%2FJSTRqIQm1GricSKOmgoqmloecpT0fQIfACZF%2BZ1tKJUee5qlWAcrUH0Sx6TpK9fy%2BWsCtjhlEwVA3qcBFH2jieM2TNld147ulOkiqau4g%2BmuICnEJVYoIhSXEqh6Oa%2BVxWNGRptLvIP7%2BYR7bFIcAYOlJ4qWvArbnsjV%2FLPW7jmB2HFbyihnmGm45&X-Amz-Signature=71a267c64703b80a7109b9809cff3613e264cbf238f277a364d58f326b1711f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

