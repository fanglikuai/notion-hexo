---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFYF6K2J%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFzpL6anOSDEQPA6iRZtaMQYrRfL5DZ30X%2B6PfB4CxgwAiAnTlmAXYJ%2BGVWNpYrFBV7tIQfbWhMFzgkn18ubKZ0y%2BCr%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIM3N3Dy6wkB0%2BfQ8VWKtwDSIrIY1Ms59MvvbK3vGj4ogbrGP1lpAz7CJ%2Buz%2FCNz4e7aIVv0HrCmmQ6kBwx5%2BPPh4nuXmX0LzX8ln8AuI4Woy9sF%2B1ootG%2BK0hTuDp6F3hS6RDJKgq0t%2B93n0u8vyGuMlcmC5sB5rU7EYhqFNrB6ll6dvrjdk5XbVEsGwXtquRKC8N9Rv5noyTwEpOHQ08OtATh%2BxrurfIh0zbg3zQkgmitzRlFCKQTZAShT40ijl8wgPPquy7feZgWxLq2cWGVqoUrBJoLT5o3naqG0Ur%2BI9EifI8MZcYCCdM6GmX2JrYOOAUdD8cIcNTHOsQOtNzUZectGTcpr%2F3chI5rdtOi9Xqi5wQUCkKsABAx8iyWqBbx%2Fiobe9mZd8rtOdciUED8buGa1YGO8KrZRkEmbIpMU05OyOWjfkJzoK3VjPUAy94DnmzPPHnuxEpT4u95cfRR2KUEd1wAoAZVvfFaDtFg7Hgd90ak5AvD3SCdD1pV%2FwDGGiTRge5V1S%2FchtSi6ecfiR2a8mlU1GOv0qIcIobOGJnYIt2x7CIFe63ApqEsZIVMKGusNoPaxNBrvFvnlfHrmH7vK1jqRTNHS6ipnmUvyprWJX%2Bn4wlejxs6Wc60wIvSyuxn986ahBM%2BszMwjdqVyQY6pgGt1K5W1ANko2lKRY9XQjeskf8T%2F3H8kTbA1j7nhl5MwzOBLlNtDZBIeyymoMJKYIc2kXtKY5Q2p2Cz1ukop1RD%2FGhr3440Ef%2FxMjL8ghuWkIssZs8NKEP2oPSCpb65Fxl0wiPXolcYa0rg9KTBZsVs4n200ayQ%2FLAw3EIO1fsL3Fa2gJAjqtEdHkvdaiLWvhriVJdU88%2BiucpFJLzBzfosci9uy7OY&X-Amz-Signature=d130f07afa345ad1daea9ce01d58abd529e33487325c44b268abcaf86dcbab88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

