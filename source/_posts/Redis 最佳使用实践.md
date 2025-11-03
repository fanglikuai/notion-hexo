---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHZEUXMZ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5HiomxDeb1fMr6eKGyQQrZIsfm64zVy59rayqGt%2BolQIhANPOjWLMCxooSLwfOcfthQO5lHN7rO0Rq88spQdQGapQKv8DCGQQABoMNjM3NDIzMTgzODA1IgzaLX6YXYHmvpNr5gwq3APFsCBUvA1ln2usHbmSYQktzDqqQbsBAlb0jl%2FF1mCgVG9uJQ2VZVkvwjRsoy%2FKiCkuVchXBhXwIOHV%2FDKqgdKlQVbPN02EQRVf2DCclKeQ9nkI5i6fgh5JoKBHYx5OQkNPtrmSTNm%2FAtBAX9fDbMKx8l6D6s%2F7bYwR1KdQAP4mK2M6viuWn3xMDHqfiFaIiCJdE8tkXFOAD8lT5RxiFe7K6%2BpNtESt2RsCdEW6YdhdTWUK6HnoL19lxGpKiWPI8nktVKO7qQqOy1XK1EAMdmIj3qwwXQFnhJT8wJ4civVKDfWfJx%2Ft3UPqfX4JmQtwymgX0uuqFEGm4NqYQLD6KFxKXYMdxK7%2F3t2zewTfJmJ3S3rvqpY7SdbAOsJoLvuDwnqQMFmAroAif6nK3baGwYB7FNSRWErKsCUnbxt2UGsCBJWG82U1JP6pbBBTEdt2wDUrPHsJNj%2F5eNaf33TdJasUkeg6MhM%2B9OE8eKhVgxs1KydQXg5bLFco2uk5mjT6sE6tjddfKM9aQyUmPo6zXwvI3cb9f8aS8cjIKnqxTzhbeAm60oLX8E2jugaAhpUzSXXg2TvJLcBew2lzMJtHPct66cFFwAkUAS02BDZ6A5GN4tGJw7VyENflkSRvCjCq86PIBjqkAVAeoAuDLVycwWp0ZzX9Vulv7lpLeBEQEshT5Lxs39dm6SuJNaPIH3d55HV2dfuwx%2BlZ0KbgTEcKYEybpQvTXeeLXQrAKPPSVoO7WhrR4JILyGkVpywZNfTibhTdWgC%2F5UjYsg%2FhSpRkjFxfqZgOn3V8LgrW4L3k6stlZ6c7%2Bi9MnaxwB0HnsSypjLcUaAv96F4ft3qTqlVXbC1389QNZ7D1FODO&X-Amz-Signature=ace3b2e5c188e5df60f7a601717a421c4596e37133ca79b9512b08e805606f73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

