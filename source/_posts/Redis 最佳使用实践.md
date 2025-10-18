---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WU5I6XDV%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJGMEQCIB82Hvsd0A0AdtiOoyWf%2BAXgeziz88GYXFZq5RHhocMwAiAJ0urcRrp7aF3pPQVwLZ6bDDz0ERPFDhATZWu5pCwQIiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FovMEpthTw%2FqMItcKtwDfIJ%2F9Zjk41%2B26UQ%2F%2BMIslwgQkYYlwCu2YjJnaaV1tSSw8%2BINl73R%2BYlJC6LXrdfLlAuhOe66B7GL92BGmqxfyB6BAClBKmAt7V9QuRnXzNzgdduPafcKZOJnDTKUMCfgXaDwGxzc1Yfef%2Bk4OmSe8Q5L7xN5uOjIc4OM%2BrB%2FYHyjQLXws0Ip%2Bte7Kd5LtUYp8b29gitj3inckCWsetJi%2BbiVHfFSfZ%2Fh5BB4NSpH6pnVOk4crBVSsnwJ23BJNWXIh5Zt0OIzyImX0u%2B5QIiJP2rNScgCNdGx%2BWKquii7NdsiHjDDEvUIxh%2FTPov2rgay%2BmeND8oNXCyu7nKtOLGMj1XRWvxcEhADEA1xvyP8tjilArSb0gmUOsX2cNHymlmxmUaCZgPo%2FGnIIa1ZK1gYrYzUNoMtPJgZqcMSfp7%2F4W%2FrQwR85Cuz0UvyBuuIdyoQs64ERphi9Rn%2ByYdOeI%2FXPLHXTKAan76KsZCNZWRK%2BFb1q%2F8B5VDqeWyWm8zovm%2FPSAh2t0Ahf5d7IoJuIaXq7H2y6Z450h0IN8DdhEi2E5A%2FJfcZuNespvCjSBhhXB1fvJugeSAaAW1mSThiJS5%2BAiLLaltmIW7ZGbf1sVUPaZPnAmkx6AoVurK%2Bs3Iw86PMxwY6pgEegSs88mSNil%2FFbtwC31sdU5ErXCFSxhMtvw5SKaJt15%2BNx0ujf9qIKwPUfAAblnperpqwMXkmfRiMiskCtTiovKE1F6kAftuhePiWFk%2FDvE44BO9UL7rHviEd9RwUQgajE2QDwp8jCTA0ne7KrC6hIxZQMfqmOWR6U3PTQSzQFvpuP5gEbH90zCHq5%2Fuaw8DUqzJUztSyzMsEg0mktDNDxUw8KrWC&X-Amz-Signature=4cc4bd46da817c73a7cd15e5b94403448e93a71ede02991bba3ffab335867cdd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

