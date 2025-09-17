---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VE5EXDVQ%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIBKwC97a%2BFyV1MwKc%2BoTggxOCRtcgWtUKEJPnxEMilNDAiByX1AaJZus6F1DdxLtJ0l9wV7e6yF4fvOSQowdZYk%2FDCqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FyPoTzBcm7MN10ZpKtwDHzEkjfvrOd7Ota2es79b43KKO%2F3FnDfsFJEfzqJxwjxy2S1xpfRTCdu51qLwtJvPTuNmcq8aiLO%2BPlevc91Kb1mXP7KFQNYShfRL%2Bu7WMMJCJQoW%2FUGEvqpfLAh5B0F%2FuNy2K5WNY0m9VZud9sv9Z5GQy282a%2FhpwO4VR6kTd7522Jf41XjSkipYI2eZcH1FKdScQA26vQOAOQU9RrE111SELvm3wXoi7B7c4rbnnmENSNPbXt9%2BM8IewmAAIpBZormD0U10YkanSVlTizrxWgwJxV%2BwxXLKrbiV3ZOfsYW1V%2BnwMz4prdwvXbewPRfnTJsRGBPKJRHgFa5Sn3yJd5j6DnzOnNsS6P9NPJ9OXiUVbLcJ3CTfsG3SBntOpy3ycYCIo8ecPGehk8rq2Jn2Nu0lb%2BagJnlvPlxyxg2Vgj7rHzmbjAibqmr3LBUlDuy%2BjDeRF3oqv4BoTWCiMAoVbSWm2F7QpWsLvRqFpDTJC%2FR%2FV7b9%2FFEogAehvzv%2BpVR5x2CZ98NB2%2BlYAmEAKHguWbQe9zIMMWNuhWPzRcNwmPk6GqEV5KcPVIrFO%2F%2BmTUINgG0%2Bfq2hPn5CLoVYXopfMmBUPO49N3JPuAUSHv3%2B84DY14GS2zCi1AOf%2F0kw%2BtSrxgY6pgE7g%2BGwyQ8mieGacKq2SjuvvEI0yRSVd3WX0JlsPE%2FL%2B2oZXjUA1QUE5rCRa3Xrjj6vTNT5qsZgQy3rB2mEhd0qShKGUsmCgaYfQ0CQIn9UTGBnIWLPleg%2BgtQuEtWi3Z4Uyhah1e%2BFGVHBur%2B4MSXl393tb1QCnUZS0iJvYF7kXNMseBUjSdQfGq5r1upJ9uHfrLCrv63sv8bD2DUdf3SpcZcgrZoz&X-Amz-Signature=33b0a988cf30715302781291d6fdbe2b8572cb7b16fe11ef936acc09efcedcff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

