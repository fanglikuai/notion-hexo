---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXE3ZVK5%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRbPWGI1RCOan0m37wrwDMNJPnICYx9BVMF2EBbtvSswIgfCEKb3sB9QOrtGk9ZU9UK2tliRu4IRpauszNtoGUYloq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDOb%2BjWVG4tY3u1BHdyrcAwLt93OWhDT2FFCYGW1pVDjBhCGH4ImkwUHWIRgzPUgKoWNpVfDgEoUpIHbwRc9tlr8pKs8KpCmFc0LYhA5%2Fqd4HIWk4RT2deuXKpfXpGqeQmDERisSLKxdUk%2FtYFBly4IFtqvscDRaoZ5U6AA5WHO5B8yQnhuBauTD19a%2Bq2n13YYmObldLETCbtmcapWBOPKsJjQY%2Fie0iR8%2BTWxPb54Hxv8XV4tZUmwgMBPFuyjaP3X56jip14mjAqc7QqWe6jYiERmIKUNKIy%2BD84MAjqQepbugURULJHpXpPGm8mEbZiwr1jbvRF7pP8LpJcOarSUofad%2FNc%2BwbGBH2KoYv%2FFPVyJoFFk3xqDUlhKPh5oYs47ZRaYUedBu682sqdVKkzWJil%2FtCZ9M8s6dp3zSyTWevB28cO07nj8z5%2BDmQzU55IXm%2FatGYRTcxGcWZODZWKObPVDNNglqPFey8V06Ai4jzkb2AmAroEexel57eZl7PC8%2Fy3MNZLs6XLEfzEovw8sP5CPcbQId5zuyj5B702ve9eCne60rb%2BNcPyMdt%2BCflvlCrHifYbiC9DAXKmrURijjW1ygPO0OsEN60s6mDqOt5QEcg2DujKEOhiARIehcQbnapaV%2FuxVzKMkPOMJKB3MgGOqUBT6UsnZVOf5wfVXru6ybhsws1CyaZMngloMv0LtssAKTdiXDWM1ffkMPimRd9HlaGbDX5CKC5GbGM%2BjJPUp%2Fk0AdFl%2Fx8cnqEvsRKQNicOZkj1PAjc6okPSnKfrZlJ%2BDq%2B5925R9A4andZFrWN9%2FtQIz0yTO8UXG721Nt90Gojv%2Ba%2FJwyU%2F4qcN7H8Yb5aPJxWt5CezSb7kZ3UjtE86Rr8kscX0LH&X-Amz-Signature=de68d0e493decbe4ce7008435b6b403da27b8b2e319ce59ce5fd67bbe1b1e812&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

