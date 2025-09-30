---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVLHUW6C%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T190100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQDIHMsplWinLve56JTLiJC6LKP%2FtnjUJEqFs9KnujzEIAIgNIElJJvaePywQSGRsDNb5dOaJ%2BI4AJjPzFWeQgwzUvoqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW9cPhXP5GA5b2u%2FyrcA5S4wZ1IoQoa1WeljrxO5J8%2BXgv1eVeS1jgcywwokqrj4b5cTsOo9XMOBr4YwPilEdRC5m0QuQsJqv%2B5WuR5kbFOaeO4aVNB3b%2FWtyitggZADCamYuEL%2B1XeK1VxhtYOGRTP5teUi88VGkkaCN1zAFXjd%2BA1YOoqRLJfiE8Cs9rUVv6WVLsjPL6M%2Bzu3dsfThhRSEo6%2FcEEBUVw6hIDbZoqB8t6nnHhEljau6oeNWZ9Qo4ggWhuowU8lCaUH5Cn%2FBu7FHKSwk9odbwV%2BVJK0h55IrTtLBBoeiVi0xUY4let25Um2ENxxq6tGl8%2BRHFO7VK6zNj24hqLY0ZAd0Gi2OEZu4EQNdfTgIugr1BinJTiMpMYjO3zNWsxdiLdF8wSFJTHIpsqL6VwZQVC3M4ksZc080lslNvy2tFMFs5TCc%2BL%2BoXfNDFO5DX2Xa4qW98rLgqK74%2B8yaS%2Ffor2K7pejX650NUcXnKTbMsuvpE2cGFX1CgrOpZniC4q97JuCkopaP9CsjeMNgoKsRoxukiiVvA96ploQY3uMkRuYYbKqmo5L3Khhk4GOk9XWhoTdTlB9Xreh3JFoqLcJ2nALGUX2wdLp53raYa0QuQlHfxi0xJgb8fvnXJhQc%2F9eiyNUMNe38MYGOqUBZhkezkw1VB9mM5uXbeBeNXOmRO8W7TRFB6mOzfx9UiJyxc3Uh3hGAP%2FyuDdfIUVwattbYl2sIK0M1nEDwwpes%2FaXtmwE491jtHjt4erfQnM54e5APFmFZDJTJy%2BCDIlYMHofT4O4bAFYIVY8RmW1qkuI%2BWQOtarlYxiN8amcHBpSg74Pt6WPjExfGSYg1PqTdgIDZ9AA32u42UTQEqhZw7QDo05w&X-Amz-Signature=a083ab4bceb7adbcb11368da7837b8bf3968e16401c5ec206529f7f9609db184&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

