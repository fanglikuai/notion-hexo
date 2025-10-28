---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DZ3CB3S%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQCWspmqhWkhP0%2Be0DAHatLD7D4tFebOC4o%2FL9vlZA3JyQIhALYJCY8a%2Bx1Vh1Ff2blB12q379hQzgyFr8XJqcQZLrviKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvZzlPdE8q4VDyOcoq3ANpXmL0yicAU42d0T6SZ7FLclvfEpADU3sXZ5%2FrIZ%2BA3k6%2FpMDS9fC6VhyxqLGnEjBXK4VOsHtI2ayfIS4mwahLRWhRWVhUdRq%2FShT1N3wpJotDBfmT%2FhGMbhkHMtVqs1YpM5QzLBvWzA7xdpsSetVqXnCPQyT4CgANtYxxXXfEBFYl5exzxIX6bgKN0p%2B4FYKZKde7noa8BAirxQ2%2Fj3JCyIxkVmIr73Rq5248S7Ub28cC%2FWvXxwuj3iy3A%2B8%2FksquhlHHdb5x5jZHQfEEYm6LgUlwaxSXmwmQoX7Z89s7CNrrhEosw3IZwqPSHc1PrugHzJVUd2F11psDGhiXB8Ek6RaBIpa5TUzkqSE4wFYHUwrd0uqk2vHC0t%2BSMROyS5hWtmqRonsAELDnuKrZI%2FlG2ys9nM2ea1smIR8UYAzWsZZng1tRLYSAsWS470GycgxcUFNuDiWjgSzGn%2BOGKgK%2BQTAtfqAN7J2xaVuw5%2Bvrft8Ofo%2Fm1XkoWZwZ3xLADppPXIzrenv3RFvw9Dz0B903USD43gY3bDCLEMXDgePsaOuvNGUxMFJHxLI9v%2FbvVOS%2BXyblh4%2BMocWoQnc8Xs4hEL9KElsIAraggKx8nzhCjaadcGFlmvSE%2FQXnuzCGhYLIBjqkAdMsc%2FwVZcMKMOneLVSG4aCGfa6zHbwnRuC2c3xb1WilLa49bkLKoZAzNP%2FXsw8yV3C662aRSy%2FcQfsZXvBlJ4OPeX9eTQcPIOjfkerM9Gi24agL35Awjm3IvfqAbFlkTfuvnMEhBUBGmQlsPDER3rtcqTMgS60usfkkWPSW%2FLSxd2651CBjhhhBxmQ4beibmlfEYZU1KVQDlK1TAIRrpB9tBvlt&X-Amz-Signature=7be37baae3f5e12817ae36cc710911ccbd665085b71454476090f9d4cd54b7dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

