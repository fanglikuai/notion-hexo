---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUE5342A%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIHEOlYTlqnM9ryNzdGfLjW9IeZDzRtBGZaolAX8p68mjAiEA3La6qxz7qkrdUoGVeVSWArjm16MqJkRXtOTNdpWCEEUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsU0R6P0Qriv1oCZSrcA9MK20qPijQ04u1wlHU4dB6keJk63CLxzJ14trb%2BL07VsF%2BYBbHoDPHN3QN7Gu6SivAr9hW62g8%2BlVMU9bRByM%2FmLTMqkCLQx8Wb9d606cP5dj9NdmgY3vYznE%2BuW8227gAMPjPug41nFxUEJ%2B%2Blk4G65adzOElmbvmq17dwEKZKqO%2BFYDo99Vzp6f5yt%2F2bnbDQm9nlZymT472W5RKu6IrE%2FW8rtRx%2FcRTWEF2ApRgOFsSnrSay7R6%2B8n%2FhgablpTYmm6s4DU8AFsFKC3hWIHoDm2I%2Flj8dHSRRXGGveONRQ%2BbT2GcS%2FOIqGny1mDhZW7KmS%2FxitiFPn%2FqUxhx6AHzXBkCJ55r5KnF2SJuCNM9B0WspOBCGYzu3itru6ScXgSuv3N41XO%2FxVrXrgzh7%2BEx0Sa5%2BvsdRU8fuDvxaT%2Bjar3lIa9njpxJT1%2FZ2KyTVt0gBdB%2B6MIIU4UPAqhi0updbvpZZu9jXQxESt%2FkUyxPrUQvZPnft%2BM%2B0LeixCeCkdwAH853K%2B%2F84K0XhloDRME5JnijIbeqefhqKRzKG89ElPVQo7c5undePpNkUrBnhSbc0VUo6ujPA281C%2BTos%2FJlp2Yl276fOPxlDNjA4M%2FQA8Uav36%2FoXhTiWekVMMCf2cYGOqUBhOpVDmMIAwThsuLjc5z4kG%2F1q4tBXVOT%2FGTD%2FdtZHKxSjOVjAVczkShY9X7JioayCdF80YH5GV6PGRaMy3oJZFLhDXkbTCmwPbzDrLxkJCVclE8PN2A6zmnmIROOkkgRArdUIyrvUbPhMzv3qD5NgunHofLOBv45U99eFSctGNflomfopy0kPBH4XuErH7vvFrvO7ro9vO6gZVJ%2Ff93T2SlgxBkl&X-Amz-Signature=60566e173a4494e8973e242b0aa9251eef07ea77d15379105f8996c07a0d8956&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

