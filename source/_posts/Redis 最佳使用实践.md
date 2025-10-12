---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX24DCEB%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T150056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIELWZC4ke95F4eMszaOpsLbZXUUBvrUNfmolEQZmns9WAiEA%2FuTnnnVPHTZ1BVphJyu9Zt%2B2W%2Fo3jhYsTERmxnckM9gq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGNLTQjZJyF1ah1u2SrcA79Pmnt%2FWr%2FWytEzuYb7t8%2Frq4b9grr6ZKOqNzTE%2BvXEex7Z5D8KHeISgyh5LLKR%2FU6Ow215gw3fk%2BaSVi70i5v80Qsl9USKu7kt6VYyH%2BA79%2BD3BUEokUi0XyuqRHz3lznZzdfKNzC3Crl0Uf7ufQWoQNkGXIJDyXaSzD8Pv%2BuzkQKNJyJLSwDJl2bIu0wK02EHlmo9QEKi4WgE1nR2SDUEMBJjO4sp1dSm%2FLYeKF2ADPQByRBsaeo%2BoA6Cv8Lv%2Fr9qTXbDJTqA3%2BnqR0PhrNfzZdXANavmS9KO954R41nnqICn31pI47%2FhkoEqws5kEygY7rxHWecVf8WyQv23AHzwTmBH8ckuoAwWYPtkR520xFIqs5pLoNDFLKLOpyTbq5cr1zA%2By5XwInhcNHl%2F%2BETOG2hwQ4qe1rX%2BdK53e%2Fmv2g8PqESVX6A09Z5H90lZJaI1oar%2F%2BmfYMyITzW4XZ5KJw0FTubvqa%2Fr3KuFsgbcIrrlesplhS2ewkexO%2FAxwYt3CJHyqYEnBVFO%2Fy8JFV0NS%2B6FP7FWz7nWp4VCnTShvaiD04OB%2F8YG7XvfXyGgN1gV6BsmGALqT9PiQW%2FGHrVQmfATZp39Agk4y7FcIos%2F6TY1Yx1GEQRLTYNySMPS4rscGOqUB8Rq%2BjFD4jqIlfkYjxZ7og2MeLnv3955WUyGVhsaQsYvEZVQjmj6xh9szFOx%2FEvBNLGhwf1mbBa1rkpMlWDE70LvtDxmKvhbGZyqAw8Bm7nIuy9tSb81D%2FB5YoPmwsC86%2Blt3yEJqVj22o7mEs1yNh4ME6NJEbViIPQWCZmobuzz1RUd1Y2oIAn3nQrWRnBr6REqkZiEkHFxEJhPSm%2BDjdqCbPgPo&X-Amz-Signature=b3f6bda41c2d5643c34ac106b2f65d6a678efc755197af006379a26b68d82d97&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

