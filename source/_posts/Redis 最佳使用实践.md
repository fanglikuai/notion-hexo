---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTPUKETF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCICXVA%2B2ZSXbrUYz0u0rByrqljINFJ%2FZrUNdO74e6dj%2FBAiA2f%2FaNUJ0aVqPPw0RMJhuKPAIqdnd6qU32xP%2FAB4TwWCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHUIc5okV8%2BUsdjnLKtwDi4qTkWb6AwbxK805ibqUcttC4o9UgceMXKfwshrpWaPT%2FmN150p%2FnXGY92gTJETXJuLA65jRc70hRSk5M4sMdGdZ2WwdmNm8aq4QLlH%2BKF%2FH1feforNwcL4UjSZEGcjgSvbJ4o5oC05g8wXPK2%2FGcNEgIsQnfGpmskoxT1KL%2BzJmd7hfJ%2BmESP0aU1FooaoyjglDJ2y%2FWHsecaesw8vckQ7bP7WBc7G48wb1gpwjdOfkuID0r9w1AUY%2B0L3Kl%2FWlzOPI5WJyFl4kgKddFGTDDWSdNLjGERfqiIZFK02bAdQCLGfB3mA6vJF4V3w%2Fp6xN9HQtg%2FfD4eQTpL7u2o%2FauTqn25wp%2B4oZIK1pLr3fYVeSkgYiWzlKTLLb9SyfEwFWDUcz%2BD5prznWJJbEZSa52MDYhD2gieUaCcdIL3holnCV2IWkP7W69OGwrj3kWaJc8YD4h0fkkP2Op%2FWG0Si7KJYEVB8nRMyv37GIzCj9mXszN3ZwzPO4UB5EcdTuPV6zZJEDUCPVvPCPTE1EwA4zjVfDNxpizvZSOpV2JzH57xIUnoa3ppS%2FBV0D7e3%2FPncOKd4j0iOmqKwNnQTMzFgGzZtE03If52g%2FWIBXks9LIKXkQS0B%2BdPtgae3Aqwwt8jsxgY6pgF5ykxfuI9U1JehHWoqHsEzxlXUI4ooSsNGCLFQ1%2FMz6LR9KIlzqMzIVbqCRsPZqT%2F7jqahwX8bfD2%2BF6rTVBiQlBryjVmTD4GbHwFfpBVeC0SI7NNBarfWNyAQOvZMkbnMdAgAHWQVnssctPRPCWYL%2BwdfLY%2FUyvZmfV%2BN2Wlza4tZXpE2jrD5FVnfgil%2BDbMBfHBuHL7aw%2FV4dy%2B5J7i4J0L7kqy2&X-Amz-Signature=67fcd387f07ba4c1d6b02361cb9008a960fa4e5cb934aba33eb8cf1f49ffc690&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

