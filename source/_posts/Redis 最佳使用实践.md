---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WLEZLGXR%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGQZ5ljXW%2Bdky3Ep%2FUbSuugj6i651NzmfyTAg%2FAHOP8DAiEAn1sIQZVYX%2FyWi9EOzr%2BV%2Far%2BS8E58wd0pNcYYR%2BLVFkq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDLwiF7nmbwvlYgb%2F6CrcA4QmNwhr94vesCvo8eT30Fao6rKxel8wtzngpaM%2FckGKDogdtC5HsMGxx8cUtuVMDp2tpLd0Ne%2B0wylF8UF3zbmM4ak2zl7gvOfD9pCM9djMKJxSYC%2FzqHtKGeOM5NhaszVSXYSt73Zo0UFftrLw2OFEjypTuX07GkkCW4tZyh424zJE1PNJoc5qWtl61nsYHmxzc2HghqeE%2FROGMfvIZ5exTqEggbV89AnpO09yjU9qz8rUq6wIUDXQkAIArEqQVYKr6Tksu2kaQS3%2BIS6VzqbdHfHLqTZwtrWFR0HH5OPp0TvH8xpzt9gVUlKbFFG4By%2FRWUmeqpvNagdw%2FXPAgP1eGvtzwwVtxcirVXAcILYHNwATs3xkhdNyzGPWq2CLCcB2lCRVxR%2FPLyQq53xLGZL%2F7CYzQASypecJD8v5R5XyfNBim6nNedAz3UTId%2BdXk7IIYbRDfkUHZyiUpm%2FtR%2B74iv4y2HGZvVSNgb9%2B32Y5jKz4DLeSeuH9E5j2ZvcFvgxz02a3I4Pmf5zdj6lm9O%2Fh9DFxHqQPSDrbhmynhUwvK8NFZYst%2B7Yb8ST5%2Bt1NLRHZjeQPxbi6%2FEANYYSPNOBwB1vJ8io4pKstyUOy6IpoxCKNeXuEirWZnHzPMOKXi8kGOqUBajhWuxZxWKkZWtclmIRE0Z%2FufYaJZMFjsYQFJA57HM%2F6X6%2B2oshTylucjvamjviXPIiFV6aDXlaH1q1GaWQn13%2FUNmoUrmZUp6lF8GtmALCH5FAsXIWuXDW%2F8QcNWQtPiU09Sze8QA%2FV7sSOQDiJw6QOc0kx9qwKG3ZqdF0MFqEeAyecr3WZdzETlVYqWNEp6nw80uHZCvYHkQk65gANwo7HS5N0&X-Amz-Signature=39d7446fc08f475ea4f45d2fe2d28250c50ef8456441cd45d37b26c467bce1fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

