---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IX7LHLG%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDw5DVeWCs3s6MaqR%2FDa5meA0TOYv5o0uG5TH9dKF6EyAIgaC17yyrIzd9U2vM0iEQD3%2Bqpvdl0ojz5Oq2dfcUE0sQq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDDUIjI0Glz0kEMMJ7ircA2FqjQ%2Fw0wCMjaRN9q2krv%2Bgi2XGerCYzt3Onh2jVyV3X298bOE4IgIyyvnd1b7cfdTVZ9SKonIxvgLIYGv9FV0cVeleHXKPpZHmDenkqaPLPfknSwTYAeeDBcY1lDKLgERImVV91cG70lGYKJdR%2FeSthRxLypdveUxGqTNsrEpJd%2F2pNCOOatPR%2BzcLtyPhzkqbq3pHMniygYBCutFe8%2BjplsNSXjLEr7uX1IC7qydb%2FU249Kr7lrXwdxXK4smN1pDyb%2B55jruMR%2FXcOMFEwHoCrQLcl6ic%2BlX606kJ6LqPKmpoxmrLV8Ll12sBtcI%2BdswIbaAL9v1d63vbgf1YaNmBT%2BB5JlJF7VWXT8C0qaFhYz2%2F25pg8ZCUVY4nVcILIxjiYSUai0%2FJrjFVQybHlDxVju8cSmk3sT0FFXm6W30tReOqCZxhfuw3AQ0vSDfaTbUrR2VrILOvIjmqwA4eOB%2Bz9qDFoiL2xSsXngPT5wg%2Fs0pn%2FAWv1T6DKswmcF6VR8zEkE9Tre%2Fm8mKYz14hw7CQ7CRNS3tFUam3zzpH%2BLQ1tOT4A12dvtIXtk%2BoWBSSNKz9abbc%2BS7NxJyR0Cy0FCNACkjMbx0VC7MaQBbVlm6O6toru3wPumI7C6RxMKiy9cYGOqUBG4MUqYUP2iwdsjnFD9MXZ8EiYfg5yTgOvPDYdMhrZoHGkL1HAi215TKmN%2BM6wUOkD6RytYrqGhwVQo1GjPXE91X6vwF3QmjyhULFajtxLfmG7ImhJL8dX4IpuPu4k7YZtGZ3GmPm8bszjZ1oxLgIm%2FpJednISElfcEFtug0p%2BGzr9Da%2BYGz6RLPUGH1IGF4OGmoAlJFjkQ%2FXtvsjQPKbIHM89TUa&X-Amz-Signature=20ee9f8b8c0de7713bafa5b7e366151bda5d9934f38ce0b449e3c39455c7d11d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

