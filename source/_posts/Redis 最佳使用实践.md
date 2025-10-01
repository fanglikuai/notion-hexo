---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3AXHV6B%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T130051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIQC6%2BmatTIsI9dGHgpDinQyHL5RZOGRXWTlXQkWV0%2FiYagIgVWhb4PvPmHbKjtvi4u1G5c6QhKjC%2BiZCiAO1OCTvNLsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE%2FtMSvgBlFT9m4nISrcAzWK9H7LL6dejWLXed1qyecp7RTtS1dzYq4KxixTsd%2Fso39WglSP5Pd7nHiyO1eKt4qIjW%2F3rev%2BbvS5BMo0%2FgQN7kRtHUe9hahXTxlAWWuF%2BcVgl5CNnqFMZ%2FxituBs2ZCKLHtkLPzR1O1lA0ibT8riEyMEQ%2FAs5YjuZxS2YNN533WebcvRkqR3jTzrttz3Kt%2F9QTAikgH74ie2GvnqYyLW%2B4hDC7VBh5d9rDnj1aCFMvht9rfTCYeDj3Wkj6NOawG%2FxeLgT0rO2drh%2BXgLli90jkR8r%2BUL6e%2BQV%2FVHJrtz%2FWrF5X68YjodQB1HoEx%2FHWUxBVu0yllfSxQoT9CnWU9TgugYCfCtQPwjC6lX6lk04%2FmZXyVPK4qQP9cSEPotvn8HTzjhBtH13A%2B5AydDtHkcwyDEI1k1AbEGmsvwnpZQAReRVcXV1xLsStsFH4JITIrPFQh7dV%2FboppJVI2vQynYK3wbLXfQxZ1DqRgfphRWV0%2Byldb5aOSCgOz%2FKTCBmPbVWaBqn3yL3SndVeCPjhLYdYSygKUS7gKaIxR%2FZoE3FjJvx0sdOtZ9a5ESYxpuLQp4WNTH8ZstRC%2Fn7GktNzT4Un%2BkS7GPeIvMNsJMmMgpWe%2B37kS2%2F462dWn8MJq99MYGOqUBkw6dGgZxWsBqCNj2ECBjL%2B45WrvWSV5alZZ2lcppUaVXN5Stp18V4OLhpwmRHDsyOACC8hcCJyNGg0N0P9tAy8zHoVPCaoyC9PIP4CamF4pO%2FLCQD419i1ZZXqZNZkBFs%2BXI27oXgvb4aY%2BYPWSI0qGU9lFkrU1on%2BnCI14jwkIifKpXaiksL7O1AlwU%2FHWeY33Ij1%2FpW7mfFKKRw%2FIMjsLad8hz&X-Amz-Signature=10532d49932eca089360bb15aeb7324fd307fe7973efceeb130cae1b9be4b7f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

