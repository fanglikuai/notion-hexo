---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQMVEMZT%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIQC%2FrYdQlXlUBqQXWpD5pQUgxD8iIRvrtBXtftOPg5GjZAIgdDtZYGYpFAruCecN0OI89tXDOR6DU6YQuqVzGN7t3h8q%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDJTgSwqQUPIlEjAurSrcA6TNDi4uYvqJMuQ7hJcmljazrhMKvYmyCTALPOA3eZsmuS%2Fi1SvbWb5j7ZyFXVNZuUhd3D2i14nCWxtjfAs99U8rMabWWwk084HNGQTr9%2BfVb6UgKrBkwxx2QNOyZUDDcBwGPSfWeEBHPQCJGhCRahspjamo97NvH6UGuFQAKQj1DYza8BAL7kt82em%2FHf7uhuc9YYafPOGFRW2oPSWCr%2BzlzAyaun4vrYRc7jd%2FhOgnZM5FJwqOZXYM48IessDjzAWv92JidshOT5lVe%2BClZKRCd3rhlEfzm9vLG3WL9DrmyPmiw%2BUNSghJ0Mg1TzSMf2zsjw0JV7OMRxSGab9LSfnXybudBPQN%2FjTBKfOftR4lwQ7aIPZBtSjVq2LiDBZyfsi87LRV92DwwOU8iiphRjfPbCapQDqjZdTGrGRFuW84KGD%2B%2FgaxD3001vHnbv3enfTwdZwltXXxWfkwKtJeuGAaSOHZeV2lYO0E2n4hsUdTz6xoF%2BLIOqrGUM%2BKRsay%2F4iO3qcopkadp%2BiSpCXpsldTkS7bXQGKPoUJ3WYxDwD5yKs181FSlLSuZLPZP%2FRZepQTyqSZnyQQF%2Fpi6rO6HlgGDPqdZfAf3czUKY8gWHIwRX3JUUL5MHPR1fQyMMK7jMkGOqUBrd8953CX%2BFzBymoeuWXuN62kpFnRjXy2wnAsmX6ZAQw6lGNLyRTY9emztVq1ft3YEuoiqeTPL%2BJPu09n0Bi6ggWLaw1noDYVjERYVfi4JOO8R22cX0GRtdGmKxrTfahC5oJ6uYBx1ebmlVNYf5LlqdCYwFDvI0AsmZirJd5FiWeBw0MkyZA5h9p7ZFdT7wBule7q50huuqXMRJlFaXImahIO%2Be1q&X-Amz-Signature=b108a7e634825090f543a42bd9a7c0e26b7099031cfa2b61931b83355801d7a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

