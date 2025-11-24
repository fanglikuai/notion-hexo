---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TRXVTIX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD78vd986mDTwYniVb5C8Du6Rg1gN%2FaohLYWaobIUdLzQIgKz017NuC%2F9%2BIpdNDGpFK0AZZmE%2BvdU1YNGQpTxILmJQq%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDBPSRZsRLoBPvPGTpSrcA1ZtboVYrZIZcUYOVtMjCR6aVo2TMj2Il4n9f%2Bxdy5%2Bg7eMl5GQ5zSuldOdnSgK35G%2FkB8KHAlGVwvA%2BXcyVnVoqPvwz%2B6fHM2T3p84VhD5qh7Qaj0hMwktJGAoflEaGC5%2Fdhbvh6nWpvifhbULe%2Fo2BSQBADCaRwLOc3r2tMt0f%2F5ORVsKLL0jl%2BxWW8i65alz7qLfa8MIyO1npO8oK6CbsKXerw1qpikLui72KLPa1H0obRPFxV6WQ5f40TtnuvkV2ys0wVGH7iM2aoRJra6%2BRqjtubSX1WMJLUY%2Bb%2Bc%2F4AD%2FwBbH15aNy6kwdqXaVimxmNxqluQvh8NBJyC19XYXbXlC%2FTrDAiBmizHLMImzkilkfP9Z%2FmHOeB8ETTWtPfv3c9ZWqyBSTXGsZni1itYIOOE%2Bq2scmGKRkhVNil9r9oMIfuHnB2HF3zHnCWn1YfCvOPj6R2KHMSXow6w8wSmA31TWAhMIUqrdaMrIbv8MJBL93FKlwOlmXoxr7T9CzG86IZblhDtDb4t2VOYHfsXE6%2BRma8qD30MKAf6T9%2FWwV2zxZ5WuNfMqaK%2FsD2FMNkkKP9aaTrOZi6JZaEkuVsXrsVIEp83mshsCIAynUthVxVUV0zlt8Eb8VAzUsMNLJjskGOqUBGPrVUqkmUPn8Ny1MjswVlr3RxqmE4ZdSKGyWdzMOMfIQOHemAm%2BV184vxyhDaIjN2QWSjxkJqR119OaS9BdHceKQQ1WDvnGrkpwiy%2BGCUlRoHzk1HnsfoHrIKZVzsrIdBmX7VrIbHpl%2BeiUYsIRMBI%2BuxcUzmIOHyXEywRP%2BiQYVlCWROkJ1U5jfEydvSFJk3giWpMJWbVfS6O5OOBXleb2KpwBc&X-Amz-Signature=e777930ad271a85868b9c6c4f732fcf3db5814cf9410653469565e31594655d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

