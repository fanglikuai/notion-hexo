---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQVZZOE7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJGMEQCIF2CbcGO9az%2FRUREwmdwFayu0mNt8l1ZjZLChKZqlDflAiAUGBGrVFIEQwt7FXjJtzCdO8453Wmd3zUtZtcZ6kh88Cr%2FAwgJEAAaDDYzNzQyMzE4MzgwNSIMKF5ZFQs7tPKFpO9DKtwD%2FLclcr8zkql5ALCv6%2BKyoadAHrQCCS3OrgcUwB0NhnPSebL0DhTt0yND0dZOECXmaV4V40ZPvgu7MCxfYivzIa1va3wJp%2BjG7C%2FS5to4b5au4Q1BeQLX7g2BxCVTYCQ4hBbV2K4L3DsN6bbc7QTdGkAxJfXJESgE7vexf72VqB2I84ODowrN9btgND5QEVtGVa1QPzDiKPSENsSw96rbNzsArW6riVJSTywjaiuMC8kcy9BvqfVGPgOWXQTM6Er2LsTrzMammgN%2FjX%2B9fiEk1JXIMSn5T426tfiDS47z36tpMbyL3DYkIDkKpPVbymiGg34pFfoC1z%2BTcpiFeYDbYFhKe8mzU7x9vLrUWzsiaysr2NneIe8whhKKM%2BFukcv3nZOGHZ%2BQ0hZEVQTMVJWq7RDZNcEQP0iCc6iXanB%2BTIOjOTCVcer598dJmviBsXeGNXE4NMbthMgaJx9t1%2FpMYmMVjjU4q%2FgxxeQ1CyobF1MrNiVNIqDRD5EYa5B%2FKVZQDINtrqY1N8hiwUzN34%2BPx1CwKTkUKJHGZyTY3nd9pMx69kRUfRroJVCBj4vtodshPBSXSVazlo7eWNKKYsdpZ2ge%2FBfcIZVPK2mveHNE6OF5L3vIioYsm8fMw3wwi76AyQY6pgEYCWGpvpgnVHXc1cInGgKaFd%2BXzVf8icr5xse4QWEO4XKDkMDrI%2BGThkv2vAHOmHCVxbz%2BIo4R0tSL2Td5LL%2Bc9sDxYgSTcIvHMTkHntoa5nlu5HkoY6Y9DkeBQK4Igge%2FmExUAWgRZqHK8yfBODSx7gvTNzewmTwlO6G4sPww%2BOGs05I%2BHTuVzogCUztw30qKG%2FZuV5KLP%2BGcwHwBVwOAnuFI1VW%2B&X-Amz-Signature=d21963823724261f2a6497fc4af72530ed86283cb40d97821674bcd7e4b419f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

