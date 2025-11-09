---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBJPVR46%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIGYb%2BslfyaAFKs9MJYV9fsMykDmV%2BgGZqt3WmwwtvflnAiEAjSUBfFvcsggOgZvYSoJgvIsZPWhdLwAmDuEov95eKxsqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDARIeesTFxeu3icyXCrcA%2F7azl4XNJRZanx522pBD4JpvGbUMk6Z4PNP6zH%2BpSeqq25hQ41OfBs5O%2BAXuJG7%2Bofdme2JlBBMZIqgBtxjPl4nXawj%2F15aAx0iBnOL%2F5C7I9HAUf4S3HzjjmNad0MwBffAfzWWu4I4PgLZx4YfgmV7s2mmFI4s0YokitnNEu1wqTvDGt711cZTAfrQaEd0rW0LrIBHrHo%2F5slTs2c%2BVzMM3fOoRUS0BstKUAi4esj5vCjJp%2FErLMIK41X%2BHJkb5H8n9CCP4x8aEi%2BWPOjpazqW1n2w5FiYd0ahRomgKuCAzxSN9%2Bw6IyGMVbN3J6EVMxjybxkAPMcgTadwzYvhUlV%2FifVpBoFhHOl0uinHF3GpufdkIjDfWREspKOc0Mq9Z7mgxJU16c71pjpcBKAUKasjvc1hPWYOd6qNqHNKEg0LLdkkyhT5pqWmstV2Uktl4Ocn9aEgAgq4ssbVEs5SkQXDYMGNfN4B90O5rLPmynlXuDR6sLkQlUO7RG75a0q2H9F0xQ3x1lUB9Y6JDSefjOHeU%2BXoZW016ryhFkIqfq20JmBoB2V4jxq7CG8PAbUhJ8QROF3UDrlB5cHOQvVmtiPCGO9qZbnKY%2BPny1JQy0NkYUgtKAwrmwqD1gqvMOeNwcgGOqUBn4xm%2F2%2BzqqNR%2FGwS86lE%2FTVGQ75%2BlHXcbISg9YeA9BrZz6KuERupSJA0vK9%2FoOVStcictvJe1s0rtjhg1rCDAWzWwfQihr4%2F9LcEgKeGVd4hzd2oX8I6RoQ%2FGKcDAL%2Bk8NBX8qPgZt8TKY%2FA5Mm9qVdLpTDnpB8JN4CTwr%2FDm9U4XZVA5kBW9BiR9GEYtQMidhprn8f2XfHP5qqlew%2BMNajca8Xf&X-Amz-Signature=059c0c839f2cada937ae473870911db70a1b6daa8139cf731196d13b1e78cd62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

