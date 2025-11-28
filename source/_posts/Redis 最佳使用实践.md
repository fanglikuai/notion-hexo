---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNQBNIDN%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7em4BMG6rht8gYjHzvh%2BzwVNP6UFqF0hFhNYZEXiv5AiA10fw4cOGaPMIo%2FTKxMmaBIySbDDPxR5yyYYdcMZB5XCqIBAi2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHMNTdUY7TOQWTOAYKtwDZIabvmj%2FYOQ3H1M20GhZCWfKDW8Wp4kmm6Toea8tY3SD9Y0bw6%2BPfR0CC0HLXzsQS38Rw%2BYdlX1slX2zqfRrDKRMOUmSuwJ%2B8yq8LTdcJN48QhdcSy82muBbGLLO9T98tjtYJBDL6eHIgdvO5BX4Qp%2FtfXr%2FXt1ZMAMxcB7UtA2iXWWs00d3bXVZ8hTz80IDCkd14lINg1l3OEqQMESTzjIEvXXoO0eNE%2Bc1ZnJWEYyJgXiNTU70Sjeqig3sjZvstB2pvu%2FucJwbyWfO0iINeumna1z2ue0C%2B2IUmDoFKMCdGpjtdXqqlJwlUrhokHigMZUV%2FjZYyiwY%2BPi8TO4uaa1E3sQgulRTpp7w3sMi336cqIH4boc1vL1bROf7GIYEWtThoub3u1grdl9vP4Ygh1VG0eMGgkSbjn08ZtpSgRB%2Bi61NZLRiJVVm9C43vNG5YuusO1wz2Ycf5bf9RJpSzV9CRHu42nMolvQ6SQJJ6PUoMNAFCzZgsqe7vNb1JOc6WepO4Wji4T16FsJgr7c2sxA1EVcbwjUNU6VE5gjDoB%2BuUIJ6Dgp7Z4kYIum6b0HTGyO7pgIBUWHqeat1VkYQ%2F7%2FJUMnImjzczjXYvWYvtrjJXccAJ8oUdOteUSsw%2FL%2BmyQY6pgFF4eeTeWKjcoN8mUoPHiytV3%2BE3e7%2FVxAkHXvtPV4lxVsDmXWa8FkgDYvHkgn92FdJBZ61ViHFwvaMZSMxjm4ODKjz80N0CsBWdgCpyFMPeCgFAp%2Fk6hzyaXTab1FOrXxeebi3gb0OkzIXpv%2Bx6wX0ewHW4KE%2F1gi319gczZ7TMnyBPVcFcCslE9G%2FTz4Y6b9Lm02k6tmpDog8xEOsPe7hc1Z5a50D&X-Amz-Signature=347c20fd8fdc23a42755cd7c0439b7506cca36b185e26a202a79cb4847df9295&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

