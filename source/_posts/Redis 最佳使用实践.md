---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQAZJUM6%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T080106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCIQC9DK4AR3Sa21s0UgLT9scUjT16qkDCJl0VlNUB2fpcCAIgPJyTh5tZ0DistjTjt4T8djabIzcrrRKG4pS6y7gQAHgq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDO1GFLGkiR74upgHHCrcA5EYPRwOWsJVVP%2B5rmn9cuDWdeiV4X%2Ft%2FpaLiHM0Mmb80bErUsTlG1xmDcc%2B8at9b3dFmjNp%2Fx%2BHLBTgbg2A34uBhFNkjgHHr6lrJTJ1GC41mOXJCOGm1pz%2BDh8kEGLeOtR3dUFxcakDeO2mVKpMUdKw7p9UTDfHcbe5MdvqjgGgbXAgo7qRj0qPinVhhZy1d4%2FULssBVJbKbyGP0W7P5WuuhG6mF%2Bxgyb8A8kgEaAfhBQenLVLZnZEKlmJvBRWWCstHcRexsj3uUlNTwmn0jx1ciBjiR%2BXzQyJOjedI0wX2YHkbLxgXU%2FoGeTWWjYdbjW7GfJpNSwgukgPAxaxpjOueJnXP1WbdBiaBAoirs%2FGNPrwlEG26sqmVFFr2A72nmZBMJzQf8gCuT3bqsCo8VfZWOtKXEnCnzJl1J5D0XOXJXZzuS6kTHkeaygz66erSpdOCrrs29V8fg7EargjMDNssZRg73OfscCF0QQGMqdVuqLLlwCLGALPQOivFu5we%2FCqZ2LzHKcHb7EmVNGYjRbo3Q7gaddwfFwagQLzfeRlnT5m8P2O8NhBUUB88r6ba9pgWrLCug%2FRqhI4BH%2BihRJnbCvmWH6OZbpgLbZA6aoJEWSMjPBsGDfE4niJuMLuD4scGOqUB%2B%2Fh4fuc4paZqgqHNvYOnqlupN9Oe%2FsgS3mUv9zrV55zcSvrn%2Bj%2FX4xrepaUH9KjI1N22bS1iTUA8EBwHE4qBwQe6QWHlLKaF3lxoRoK7fBjk2yLxCJLvJjZVvm3e2A6IK4uTUfQGIdKUnuEm788xo5kjkbUlQGzHDCUh%2FQQsxL2M9h1368DLg3BBBbzh7M1DYVhvNxptIxhzjj0YMBOxSLjfLQzL&X-Amz-Signature=ac5c84b76918079e7a30af0f7cbdf90f2174564d7e79770e5b1dac8751bf162e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

