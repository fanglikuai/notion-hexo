---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBFQGVBE%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCIDXujdPkdnhJJfuIu9Dsw7IJz8zASSKXrsLiLm1%2FAUFVAiEAry5rfBut%2BHExez8IiK2rwJI9nZqGGaZelzFYhIWWDrUqiAQIwP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWBVpQYnsNMfaYpRyrcA5yLoTp3ITEl09mZhdlhpfKNu02uJ7jtzBIZlB5wpl7UxB5U0Qsy7kND%2FRZiM0dp%2BnRD9p%2B6UfjkQU6ObVgNmZ%2FjEM0sxHaiIvNlKtNVsKkSfYskcKGMADuQAK6bxF0h8so0Tzx5fKdP4gLDIWT1EIWmPDdb%2Fd6i8lnoCDMSYKeBsDeEM0voEoJqZcDtVDMKocBAoBVjTWkGmjHtW9glZ2JFG2voHjRywGDjqwz0TxzZo9d6Hrf0Y6KMd4WrGvUOFr863E6NlWe4ZOiKWty5DrqJHvUJj3RNUfFWNBxEGsY17QvqNoPv%2BlVuxGukzlf6LO%2BsDlWRR2Xx%2BOHIpyoypg9AhXYeqTgvQKcqD7Tms7rMXofXYxjK1Mj5Id7wij1fKZCJyubXJFSKS7iN3ygDenoAR4GBE2hLQUD780gBZ9ChOcOqizqN2XfD%2BzE5F7G2IK5wLUQpUx6oPDvBYfWHdGq9vnezn1DLNhB8CC0Gzgfy9hwpMzF6hYjIvC5aeLsMJl4wYhK18F4xq3g5LGxM%2BaAcEmuHbR2OWXRQc5eZOb1UCvvs5R3B84p7wQM4OEJZFHz7tbUGx3wemF4pFUu5Qbk0%2FQ3wKKFuIcpOkueRWLouCDdl%2B%2BhhKF%2FXxAzYMN%2Bug8gGOqUBGgvxtUkYjLjSgE9GuVz6GGKWDuYLMTnMemCrk6bb6bFxdug%2F18%2Fyet2PoGfHmLIjCAnUfwTxTjfhq%2BfXFwIcBJZ8KJ1GLAD5ZP8AOZYQ94rmKfHmSZdzfumJ8yXyASo3Uh9ModPFwYuJDSr23gIgeHX3DsWYi4mWJDrcsrnDQAF45dw6AcjYFXbld3ogtOffXp20C2SCD3H4AVMUfMliwFzQOeCa&X-Amz-Signature=0d7085a29ba9ba60897e606e5265ffce37b2a7e2a42fbc96e3a671728b32a389&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

