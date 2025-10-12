---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDVUWOR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQCEzF94%2BbJ1CZbHs2uecEDLB%2BvhEUQ%2BeiepnFgXBaIrwQIgK2PnGuBn48KSrDh1z2aI3UQimlafJaoFAncw10MWpnkq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDIDsifyxpVQe4UG4iircA8ZdTWpJexKfbeKRTVPqQxK%2Be5AtzrGHkAjETSqm61FG6PoSYJFIMsQ9uLmIGh%2BtCUdYLMwlIG%2Bjfu652mAvgoOBPhTbQ5AOCA5ruyqfgQN0PdxRlPmapakFk0%2F0ZKe9Iw6a6AtoJ%2BxCfemER3Qg1KP0d7ZH3kiAF6xVeSoZvMBzDsud4E%2BQC%2Bej8Bvfr1%2FlHEyU9a4DxrUULJcpNPXQz5oVHmJmupLrgemf%2BdEXwulQ%2BlhiAQezGcHgv1HB7gdVwRwHW%2BeJeIRi77K2Svem5tCtme2xOWy%2BhhAiu9ntc%2FqinMXDZoSbhuXxHN0o4pSWNxjb%2Fg8ohT%2FPwLH3eOX%2FhgbsyNz2t0q0UsZlRHEI328kIpToeJnT4dzEWIQCQ5Ih3KVbxQi6Yz%2Ful31QoU%2FZsP2zKc3wQiUU%2FkWtS1r2rL8hrWog%2BLIVQRt0SnE26hkQgwxwBNSxDIPkqT4EL3fIgkVOQ1FjV240Bru%2FDng8p%2FfQbGaQKFWU6LO6TcZQ0bY%2BT5Q7Q%2B8DDNwh3HlvGqJjZi51EHKV1Rwh0YSLEBBmJWlDDo%2F2b5wzQnBrjkQJov%2BquhJmzQ66asdw5G8NR7XZ9%2BFRD5gXojq3IXXzHWfbD%2FBXcOS7gBoYuWzKKV00MJ%2B6rMcGOqUBqQPUC1%2FmmhA2m2K%2FTIJ%2FI621d34zW3oj9gyZ4VMsIFWh3m%2FSXWg6SgLK%2BaHOtJ1K2h4WiQ6m6lN5peTRVwb3kuLKTsSrrM49dFTWKuEv8z7SQWOvtCvGKqFmdGFIGKydWbhlOWWaQkEngjsY%2Bir%2FqNZJ%2BnQKn%2BCZ7pg1KJJPLLciW%2BbSYQAg8kjb7lShJyTvoBEflsxu1U9Vwh2OwSBmiaC8tx1I&X-Amz-Signature=03c213c07d85e7219f08dc25f72e41c0eb1a6c93a2a990500c24b8d83d6ec4d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

