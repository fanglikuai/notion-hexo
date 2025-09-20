---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663H2IG4S2%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T030038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJGMEQCICKaFNvoEbLvRN6Fq1PY32pYY6EAcZofdM5SPv2SjBJlAiBrwCb7qlbd0lUzwFWPjh2kfHufkBJz0wMWZVu6J6Jq%2BSqIBAjk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbUfxnmnRUbMj3Xe4KtwD3BQPtRHAwNG8VvHiB4PiItbpvvU5iF0efsN5JqTo93tPjcGzC1TzJRG0yPOr6GxFO7To%2FlCbEoB62Ou49oDoHnl0sMi3ZQoVDyPbO5ikTS7cyceaon%2FvVWW6umnyNk0Jql7DSSOZMa%2BctG2bltMJypWvcaMSjov7k9L70uRaJti3%2BNCN8qShfRfBJ0Li4ivpUZUMHmdtOeg1B%2BKfDL2ym%2FNtNP4XkFtuSy8ybo4zk5vUHzocHl3Rgig5thbzF9b2l5Lp8m6x%2FL0RlanaoI9Xk0cCTy3MEibSBH3wdYeCiOWSlomtjloXc2k1B%2FkfPFArmlpAaVqrxp31ZPCK5o5%2BCmO9zavbmiU20R0lQyGRQox93%2BKust6x47Lfrws8Dd63EKtbLGjQtQYke7cRzLPIiCRvc6dBMPZ8yRin10QCiCwVSEfUZw5nDbyXt56QtjsmKvo38QOTYgOH9NGhU9BcWV0E%2FrMJ6WCwUbHlMewEIjFP24hDZ8EtSgTR%2F13q2hMO1PqkWNaCjJ%2BA%2Fr7VgrGnZcX6KRX7HEqvnKP5Ni3Xrx%2FdHGuIqGQuawpoU59p8zZQHH35ewp2ZxcImeBwXBpyH0%2Bszby0W%2FcCKZ8jRlP14f9zXadwI1bQZFKzlwwwwam4xgY6pgHqt%2BscZqaZs5OEY5leFWEWF7OPeIbif3YAsOiFX7D1VdaPnT4hkqe0gyF62t%2F9uLLRVg7OLrenxwV%2BbFKz1fnPA3lzo2I%2B7mfLiONIaQvj13xhfhtrBHUigZoP8XPAXf4oMRB7seQeA10XFC5xwd1hKXY6RRNgGt8h%2BkAS7RiMk8Jg3qXPARBo40RMSbxAUunEiRnJ9IBPfk%2BQQAj17P7Dq64t9BNp&X-Amz-Signature=3553a724cf001fb292ec426ef867a3481b9f03efe350032ea6a770f97be728eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

