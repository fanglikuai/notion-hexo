---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXXZUM5K%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBlgyPOKj1WLJ6SPqJZW3RmSGIwM8TmI%2Bw44F9f2k67gIhAP0cgKet8YZsaiQVkef4XrClbxLnuiqTZaW779c%2BcdmoKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyaTDMkILzinDrA6WQq3ANwfVs%2FAeiUGOKah3BODqdM409OqAUoK%2FJLHjmPCavTA06GOgFJ3z5YbexclAWctADRJ9UW9vXUG2hvnVts%2Bscoczue%2BVf9kKKIeAoK4giwXe4XhQ%2BFVvJk91Z1iKxLsmxvB0htOZLFav%2BQi1Ah0ivAtDhN04gE26Isd7JhCPUOMiMy6F3GcIL3xnJJWBsCOoRonZ0vDIk32eOKS9bEOcIkf6p3TGGTugAhp2SbhlhUy6iMyuEApmNG%2BQ%2BVBtCVF%2BF3A8wR8xKJUWz5XnMucJwufxflb%2FHoTcsNclgcsQYNYw0Ea7CJoU2YX7iG7GU%2BneBkJ01s6fZvTBRotgc8KJ144Bl2klGniLlDfKKqjyGv9uQ%2BeRWum7IGRLDirg8LdIWQk4LEfMr%2BuVSKm63RmgcKQXHGdHAoGGMrwV%2BTMV09hRQal4fd6Rzagl1%2FowlaAsQe41uSSQIUAQB2KT%2F%2FuQfyXrqoVgjrGK%2F4TcMH1F4LfIejfgZyFJNh9C7QbDoRtc7amljBjMfC912bn2%2FccObbNRg71KWr0Lr4LiOZD4Bu65STU9HwMbiQ4SKKXWVzcLAYuzGIiL63hNfE8AsB47kHeH1Q0HnuQo9wCPjybDpTh%2Bx8IjtNyMQau%2FdQcTDx%2B63IBjqkAQWvasogboXjLP3hcyI00m3PwpALuKLs6K4xUrPbG0OHUBb7W1ICOygccS6T5hLmR7vvI3HHPRs27LLStMNdD7TEXvo9DMLyDIc2OOE%2B2ZUttnFuLG68hTDpqA4C7LZjPPUylj7OdpaCI2Lt4O38a2nAxgUZ%2B2QvgUNltZO7OFKeAEVJOdgVF4FrGySc22XFK%2BwWuIbpnaJRdmxY%2BGGxkr0YcOIS&X-Amz-Signature=ad6258ef9b33f68babeee6ea6d8518ce9c28d1a1a293e46d3f5ed2e18b297de2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

