---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5HQM2XO%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDsy7rWQ%2B8N7DlSin6AUHeWJoeqsQqYbn7qdzPpUtJd5AiBSxQ0qi7QcVR9qDLZbUxkRfKPJbp1pFJVQD%2BW9TjhY0iqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO6fSpe3Ds0v7P7I2KtwDtAtZMl1wVOeRLiaPCTcG6VPYpskWt4OUEoY67iG%2FVclPDYtZPWZwPlH%2BChXoFt18Lz4O7mwOfGcvTKOqgM5WilyCxhr7somDoeDtskjQWdXliS4rkdkPO0g7i4hqs3T2P086FK8asmodkrzTy8fduS3Rtz7TvIyj3rlZI5X6o9xTrRUhlRwF3e5k9W6P%2BPdtZym8fp0BzkQW%2BLJfiDGMayLshzCZ8SYUBNPYh95wwZNZZqqzI2chYu5f4I%2FMgaCmEjL%2FtHwDvlnhrHqB%2Btkr3k7BdrDvglEwefkrucMoYvaB1iW2X0u3LhJL%2BTcETidDQhNhwmMkMKKUywZCSA2d5PsgKw4%2Fb2FJ5NoF7DsizUQhXQQxBZ%2BswP%2FnQsbUgJ9X0pUWoUmJOhFyytxOWHAKkquzf%2ByLNm2pfujEr7nEtVuV8yJwQdjs63jeaBo01A7n%2BGvVrxPZU1LJsKxnqR6TYVd%2FZ2%2B9qkGBfVZPGjloTJqW82e5wTSwuMsfqUp2pOZPI1%2B3v1z%2FHDKq5cu4d8Pg2XSk4tskjNPmYCUJPGyfuKeKVZKk2w9xcK7Uu6xXp1WxwjqiuaHNeEC0IpLxB3pARQShrIx1H1dDyo9a61jry5ubPTwKTBmilFLfBwowm5j3xwY6pgGv0stX26sGNByyQeFunAaVKA17MaUuMvv9RZHeEcowQ5cCXOrSZSf8azYTe83OIM2pQjEI6N19B5q%2F2p0vYB7uEFl1tFCnEHiV6Pl2nAIjuzZnOywIdZBW4ucwIuZBt7g%2F57GPSv3Jk7wtXlIj4cOELPH4kdG2qvIS6yipivomXPxzjY0A3WPF%2BxtoBI5%2FTumQODjCYYQWx0wGZ6GxZ2fMz1VtVuJ4&X-Amz-Signature=dee969033cc0d9e28cec534ebab102b48962144f42db3d46f1d8155c3d94cb8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

