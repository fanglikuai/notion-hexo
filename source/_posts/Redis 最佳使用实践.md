---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636CYFHIR%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCQlzIONxeeL6udL%2FQ%2F4q2cFLp8FnW4oG1I7hpb7vtKpQIhAPyq5Z9bUkcPHW1yr5hCieMaAv%2BiTwTF%2BOJ1irvyGsRbKv8DCEcQABoMNjM3NDIzMTgzODA1IgwayVUzxmy0PURuM6Eq3APWW%2FPbKuodvAKelLzQYSWIGCl%2BxfLWVYMwjH9omJSUQ00461OPfbAvxWXZBJQ6Sz4buy%2BCdshMfQJAFpCKtyGWXtIESPr2AIyq7tE%2B2eCPFixt202ARvOgjTnWr44PHPxjkXjWHXepiHTmRe4TGOwo0McMM5YFNi4wUHUsme8%2ByYGqUvk01idPZcEQXZ4MXQy6stFruJayGnMu8zdrDps25COSU1zYBNEhYo0ONe6M0t2kKBTfR73FfZzYesg7ttbSg2iyoqCaPbL39Q%2Bc%2B3athURA2ndElZEEn9Qo5c%2BJWU3hIV8kP7yrbpqZ%2B0NnMlkrzjKEwdJ5atrwrl06e45oE1TIukOG3XNqbmo3biJMD8zwK4Ud49BlXa%2B%2Fh7d%2FZTGaFDwLH%2BKkmFGsWYSSCf2rzQOqcQ4YGJ9amhQoRM1jN1P4PSIfABDvsPCJNtTpVm%2BQRHf%2FC5BReHG1SPNDIVB5yWp0vJdxj5luri4AGBrZKxhSgSMFJ2KknDR1afrwJoE70r7daVI%2BEpFWi%2B71rcRXoSVnb5Ue4XcZCqAGe04WLowAf6vKDxqrSF0uNws49vBKhVjxIwNw3TFDwkcd8GY7dvf7Feo2F8nXAvTZEgf0DbKY5XVwVqYcZX6OAzDD1srGBjqkAf1B%2FiqQ2Ct3CrDkoksGUDy6t5ZbqIersuoQyqazaxSgpipEg8fqqIZbjOH34loGP6SMX8cEQm1NPERprJv2cOJQqQDbR2Kv3nRf8JjlkMQENhW3%2FHuKMBzqUnvJ6SeJV2aAKf5RViBdQxNw6B0Aijah2HAuZaa8cKKbjFTOQzUwTLtL97gdhCKKZZtQGeDXUpAd4uDnSkSL5igjNtqEzI8QjuD0&X-Amz-Signature=535391aa07cf886edbc982c9fab25ddea002743d08f5d7790cc7b3ecf47ba5de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

