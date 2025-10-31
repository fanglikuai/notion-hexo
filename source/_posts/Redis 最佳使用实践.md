---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEZQPJAG%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQCs3kcZFPgjUKbdxyxQ6mNDyLgMJ%2BUUfR2rDzHAv3Fu%2BQIhAKWDOh%2FbabqXGFMUZSIpmPrAxCvH0TeJm53YaADkF4cMKv8DCBgQABoMNjM3NDIzMTgzODA1Igw%2BYsOVv%2FA861XBptIq3ANZA3USfFcbWH4dBm60GDT1BlGQqSWE8O22Nwmp7Sdwx5nePHJLcv5Za7DWmA4EC1qTfaTcM79h9nrqvHWc3jGb%2F01Sj00tzdfgJ4wuG9J7AZH7aaudJwpwRaaVkFnN8iHBsfxsCteqb%2F%2BmVWBTodMvjyo%2FVwssv9%2BKoC1pS6is%2B9qgsjumXH2kQFB3fEDkH9H8qqV3NeoXfHCwCiZti%2BlBPcEgDNEQE8P0yZUDfLMZSFqVNdUp%2FyKudgVgRaWaItkpXJ9FFUWxU1aUcrR%2BmOpdjuMpF6W9N0%2BOf%2FpIExq7l9b2nwltQYCvZhf7sIPEFd29LFMm%2FXfuHWkcp1cDE5ra0hvSi9FkFJ00l0jRl5HpJT20BFK9tEnhwwdysO%2FJ8xfYJlwfaWWYRuGyUPlchFiIxH7w8uV200ufno4e5lsH2BRong8A4h0LYVG4mzhRRyf4EWxzjIq3nyevhQ5RK8ET6jwXj1HsXcWw8i1H2iPVjuSRWyR1rhIzFLZhaj0gHjVXpUAM19t7e%2Fw19gn6Qma6BcWAsYRMO5yBRTLRlZt3h9DbNkFHMu3BaxbSR2foopfa0xJdAiR3SGLIu8VfpiFsJ%2FOaKQ3JX9RFkENsh51KvQ55a3yJd7vTiXgOYzCsmJPIBjqkATiX5fxtTiS8q3fs4meFRaW64nE0Fo9xabKY%2FK3ZbhFxX%2FLDb9xrwJx1yy3AMMtxGMfhn6bJQQ%2Fmos8YQbrYuNoFDGB%2BS998NxCeeQZZLy2UfINYtqWrD5sc7vH16odPgkWYovtTJAvQeQtlmI%2FjxUsoPDLhO3RpTlp5ksC%2Fxpbeh7mSrtRVZteZVVDyUJRX6naZJTchwUm0GWcuYSv%2BhXF2tSji&X-Amz-Signature=4e4fc6222075dfa794921774d7f450e83ae9d002b778244ae2a3b8cb554db3d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

