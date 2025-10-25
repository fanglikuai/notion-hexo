---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJTWVJTW%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T030059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvXaPglCbCqrsmUvlHqe7uZVQ4B54bsBj6aHVqIrkEhgIgd4G2R1JzOFU0NEMx1R2miCJ1QoeZ3iRiBJ344gR42sQq%2FwMIbBAAGgw2Mzc0MjMxODM4MDUiDHIMq0ZxD1QU0o8MyircA4T5B30cQ4u8bp8FnLAvlsx6Y8OdY5evLGh4jlxRVlelDSidWCf3HFWGVw60P3HejrVhgRUtVWJ39RTK2vxpBIHqgppV%2BrLtmd58zhBUnBwFoNt%2BPvZ0xHMk8RNwnj4Q6CyI7zzw9LXnBl94HgaaP11eddBlkM2gTvc%2Bf9%2BTBnCJs2CfzRqapyMmh3pPe0U2nUTxnLrB4vxghyGRQbwjSqa6ZZMltpRpfgxd1Y5y5xIc7sr4CKTWHaT9KUc88gaDVnDVDfH8G1LzM4xVq7wS2NPiGsQwZzAT0O7EdK7Ngm%2F0Y4Ecmk%2BBkUElDYkfp8Hl7Y%2F7J%2BepxtLh9BpilnVixya1RTVFRL%2BjK0Gt7EKmnsOKZrNhQ2nfeDcOJ1vwKksfNuFpkDa88KX1img7jTZcoV4GWSnQHcRlbpmCbwPE7paVvRiubu7JI5mOQNFsqp3gXwLeWRd8C5N35gWdjNmrzfmbewlkAhLeStYy3qR278EdtJ4V6vl%2BjpEIociM9KRICxMo0%2Fd%2Bs6IN8CLs4Sado%2Fis%2FqHR8POOVDv1eh9yfoybTo004B%2FGXyciU%2B4r0oT4w2niNBNx48OTH3%2BZfCG8qnNX34Asbe%2B1HsDoxrG4F%2ByKWd86hFbr4h29hLANMIvu8McGOqUB8LL8E29b7yJaQ33m12f221Dm5BasJhmNIB3zrc2AGQIRh8FTbGo9kZYX0yfLC4x90Qdc0AXIhTzLUrGBs8%2BA59BHM3%2BwSL0uVTs1sTSNKPuCWmj3YHberI%2FJN3ycXLUxeJcBPu%2BOQ2BLXn3ZMf1hzeUDfh9dPcvN%2BtuzA7EvqwuLOiTexNj1EZ%2BT%2B4MnGaiNotWXr%2FSgCAj1onuIJHiZWewB0fss&X-Amz-Signature=8df4b432a80c26323b7dc51ba72e492a0dc665107488ebe2d8c3f8dacfb97141&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

