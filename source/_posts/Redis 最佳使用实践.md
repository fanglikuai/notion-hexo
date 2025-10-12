---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RL4IGOTH%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9mc5BKfrVe6tEUIBaX%2FTaSWXC5S5Xxpafthrr0kgIlAIhAPBc4KnVNgCSvmHnr0KfFf8wkMt3bix6efcyRO2sGEbVKv8DCDUQABoMNjM3NDIzMTgzODA1IgzQJKgtxOAbrNPrpjEq3AP5d%2BAxJe7lZwqlaYT7GPbmQKeCHhWMNiIocSasj4lLo4n7W%2F8kd1SZdr4Q%2BDVNBu2EtdGKHC4qpLot26wTAOKxVUW0eBPG%2BV%2BsSZs%2BOQ7NRubbnCScWWbtEr9kFcnAOtTkfW%2Bdxkt0YS74dfMACcBFihWagedDbghd7Tb6vUAOHtO5hCXbwi06kIFHNZaObeLAuKcI%2FDU%2FT6WQAZEpujaSkDZWscfl0fFtPG7%2BI0FaHIndBbbGOpcSkp03XwAtyiyndAbD4b54oz4bZ597joEAs7NWLQNe8d6bsbWf1Y0t3QW5K06aRJ3KvDiql%2BnIDi494n0IHuNAA0Oaj2w%2Fg6G%2BIsZifjq7KtcTPdPNAE57Zwu1iAEoU%2FUgbcErHPht%2B7Y3%2Fvc%2FNxg2k401t%2B6c8Cvt8ndvvbjdZI0S1RdkCkXHuBdSJ09Tw7%2F4Nycq5e7mgWrQGn0qvA1h5vnWCu12CZNjKec1XutKiWiTqOEYulMcrkhv1ZXum%2FSy1wNv4Y5pElO9XUzxD0zEQJJO74grabgfKZK%2FdSKKi5A%2FGjx56x0xOUydGpRnxsqC34Qkr1JvnKCdyumyKhMOs55V6cagniPlnaPPjzGdpyZq2z%2FrWlHP7htO0%2BOxXmjwm%2BoMkjDRibDHBjqkAUA2Za3qcw%2BbXWopNMc81j3O%2BSxJdb%2FzY2YDvNfkWfJPiA%2BQWXJS6sZRD5j8B7SEjc9wI3QOzfmPpodYvGUHLPSAXxTlmzPtuPF121AHF52BgjNfNPuVqRq8WOgcxikQBxDVEL720xIfs7dbcqa7gI1d5b7r05ZYmmk68A8iX8psCcP78HQOfP6Yqjrtxr6YUZMmwReydfZj43Afr5mRhZ4IeSmY&X-Amz-Signature=429f9b1c4ac1d94b5d32d6184d836c8b4176d51ece7ff0f7e22319cbed319f55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

