---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQN25MRN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T140124Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICCGucY0xLcT6ee5GMAP30WT%2BNdr5hF5t8uQeaJe%2BRUvAiB6%2FCod79gXbVDfWHN6Er18ceryoXX%2F%2FaaR9g8xX9Fbrir%2FAwheEAAaDDYzNzQyMzE4MzgwNSIMvhHQjuE%2BULhQNfA6KtwDnsOZq3ji8vRcIwYdCfRJdIt2Zy%2BTniWKcVfCDG%2BAlUl6GQCwnq2QnloMY1428TqLmXbPvDFLwCI6sr1glFCfB57qonf4zyNdCmhEdoly0kdENU7Z6jX14%2FcIvq%2BbliFJ%2FQh8bJdxI2GvobazYdr3YdzCH41dZFZAFOEF8D76Z3lwRQzw%2BYf2fKlJ6X8TYI0Kmj0GxORJGgfRfGhkpStwejsGTrIWR0ewzKIW%2F%2FCCZdfLmlzgIKjxr7sSECq1bUq1ACl71J2S1kWjoeUTxhDjqQokBSALevOB0OACHcgsxR5fFYBLn56zL4gyMWmJK%2F8rpVxMsXcuAHK%2BqjtQABJvuM%2BurCDtIyi2PpBlggjx0cpOzeE3K3CQn%2FVEC%2FNAyDVWs4Sag0IEP4KO8x30gtfpSAas%2F%2B8b8du0QAMPNOWPa9JsEIIvmnV6ROSdePjdyD%2BW9AqAPz0Yf31s9dompewLKW7J96lXAPmsZBtZOazkSF%2FJByyi2dtz7btr02%2F2hdzD65Kdv5vQHAzp7qE9LKJT%2BIhRUfhu6fAVxJKpdQHXh1ZCPX9yI08n%2FWleLwgAWtV0k4ZX1KAv%2Fqqu6XgkYqKnasqpe7w8mb6TEC%2BW8B6jXMCTHEOljI0iHtiCErwwz5a5xwY6pgH%2FqnJjUgS4QsQCMoK%2FrH4QZKMtropLJXTHDIBmU4fNdVRvb2xdfZmurZAkFQGDFrNL1n8UEx3npErc%2F0%2F2xogrZ0VQ%2BbdekMdeFRtxA1YxZVPnfa6ImZ1LhoFGnPSOJNmLpbpHgRp%2FVhdOFi9V6hBln9Yb8L0ZXiXwQUOWUhymKmsXJjzwhsjor6MnQpX6aY0PuuteJ2QTO4K5OJiBj%2B%2B01IazsMqA&X-Amz-Signature=8d3a1e24d5043e1b233d04333e5fa584542aeb9aabcf57b03ce1e3451b773907&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

