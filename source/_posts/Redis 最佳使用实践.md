---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MIMEBE5%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIQDn4FRP8OrNS45ylXLTKcsr5eNFi6yHvzfWMheU89%2B1yAIgBP4IBoOpsfG7ghxXXcrKybCpbbB2O2NxlgwTUUou8lkqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAVNLZCP1wFg6VMGjyrcAzaM5HnUtGs%2BqBRTPQg1M153swGlclEZJiUm3vaDtRHl1Ki5C6aUia1kQNzqVAPSXWB9aGxcCfIE%2FvWbdjIX3buMFiObBGq2nqMIrWNHw40XJs8PwfrHHT%2FH4m43TYP2tiQ1dGEf4McMdclTektxWR0KeqAZHDlWltRae%2BYxIpQRI3HM1UgY693%2FeypOT56fq8Q6Z751rzkTf0QS88twANxBUfXtHN10QLz7LbqK3kCYmKjhXJTXnGnXPU3WmtVyRKOQcWb1wyRkVUMGPkcTFaF6rfNKSdRIaIJIaTycBgUwlSUm2GPmG%2FZwlMo48ajAI0RjMhQmbitDec94DIDq%2BeXVBtNxQD7XCNJlfG2YekxpfZmOXOSyDNqoRODTBIm7Ls1VTw3wHZZfg39REN4sVvj0zakhg2wxm0nt8JTAEsDzWMG6exc8RU4KP2%2Fd8G4Tbeii%2BfZAkYa1hBAdUIy1Z%2B6xuaSvxOkdTK1lkwjjj6Kv%2Fza30rMkANpxyccJNpzG8L8axqQy1VcET8etwuRAR%2BZ16dAV6e8ycZfVr17K3rkjoIj3cBTe9j1QTA6egxEDgXxGtEi7jNUwz3JI8q2ERtPqz2BA5bTtrQBwKAXz7lt4IUIb4uvIhqLkboHIMNKCzMcGOqUBuDtAzW0xx9MJlCsA1vJy0qCqbB1XBT10fPHAoOR50P5rilvXfNhCQGsz6hqiDvPzaphzywRxsNKS5pBSnx5RiSLrMJcPXuvTOeEY9LQ4sYv1H48j7ui%2FKqqZYNBBk1B%2F%2BidJF1Ezia4GsNqVxQxuq6p3oq7bBLZiV1aeQPBQiBGEA33rtVMTrijO2hbJW589y%2Fu%2BjSn8RE2SNT9EfzkvjOwS5oFK&X-Amz-Signature=109265fb7be156df5efe5e65ea305c315e75544c664c7e5e6bd6f337d6ae373d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

