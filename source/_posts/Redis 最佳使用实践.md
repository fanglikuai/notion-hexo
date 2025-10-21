---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QPREA7U%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQCn2qdos8vrKwJS48i0auyDltTFE%2FvqucL2cGnKFgMnUQIhANJlTMhlkGCuAilqzxcue3hBVoOsL%2FmZRb5Tj3e7pFmpKv8DCBkQABoMNjM3NDIzMTgzODA1IgwMEujScjVKC2sYMJ4q3AONXymAKiahWxvT2MOty%2FTwcgPS%2BM49tE9bDiITml%2BA5jTZGbxoxztOK3%2FMFj7s0vD%2BhqKSWLsKJhHhDIq6gonKqLEIboLFO5nBL6Cbi3LcSTMMD49qG0mI2BWlzlagW6afP5%2BceJwcAVGc66Oy2fc1TjVrB2yJzBvNiM%2Byg159JDK4ZTILTfSnVUMvN4GVWA5sP3GKfXJzysHWCGKQTAx3dMRCn6h%2BCJ0Uyk4Og8qeJ2drCWXyoXV2SL4wqBvj%2BuloUUUmuDO2PZQtGFZTz37fawz%2Bc7IAMwXATA46Z8o%2B84yWXG4f%2BG%2FF%2Bkt9axPLI7OQc7H6D%2FlQ0Q5jJciTVK70UHnKcF9lSku%2BzaeV2%2BgYFgst6anEHRbW93cOFrZsDSusTghYLnPPEzqZTT9HRFDHbpZTpLYVBou7C3SvzDm%2FeZ5OshIB8rkEx6r5yFosWngPVoTg26DIk4aoT480utxMXnKrmDg430p8rZIXM25J3Ifmox17JajOjYII6kR%2FHd0n%2FZpkZDBxgbfoxLwMcZeHnlBW1jACDbZaExfU%2BmqyqqTzLBNq%2Ben1t1C5x7Uu4VzA7TxE8%2F%2FLArLUmxs5gAhiR9Oj18nLrrlFK3nVcvnllZeoHLuQV9LnrWXMITD70N7HBjqkAep87lsCtnKihgzbauAEyFfuLiDomOIBHsUUkmKJ4TAoF658In4P%2BqTuIv7dSAeJqTrLRGURshkFzwXPbsAMl6iuDxD2l6m7Gg7iUfBwoUEvvj%2F8S7ic4veQcuW3odtYfsTSRHpWmgvpIcyAlAfopxWF7eVOYuLmhjI%2BHR9KfOTsrmE%2FRxPRpf%2F2%2FMJM5F97n2i80h73r7VIfzmYYsKy3asRxOb3&X-Amz-Signature=ddcb739957fbd4faba4857099ddeb56e85d451d9692de1d57ae10c125dfcd903&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

