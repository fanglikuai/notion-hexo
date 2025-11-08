---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZSAI7W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQCzpXhKLqDIVtgOCiB43tZ5X6RgNVUFURth%2Bz3ZGQQ5ZwIhAO%2FsAxzyac6Qov2IdVj0HEVAwzmOHyiGFxKd%2B0XWA%2FpbKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzRXZWGbMSF4yOg2Vwq3AMNpfOagOSQ6dmWfn1oEuhDNvZm9w8mjovdBz96fY55gd7GH64oXyMJuLsznjq0X6W5AXlPRQSUrSPbja3zU8pnP%2F0T%2B7ewZSZMU1mE%2BGqoioc1Yryo1cz3dyl9rPZCDpirkrZg9ZcCdRMx2zLU4OLOORY8NBvDNpo887SQ5KLkpWKMf27U%2FDWlZvJkCFlD8GIpg6UTERLzjx04wcCtj7kbXfwIs3am0Z48M5nyJ3ZkY9uWYzNKSGRlH3McH9k7ROUyckHudThyVKLgZiyu8vr1Z0RqtLkPLVyroglAdL9k%2FPWOFVV1%2BjEAw0xDTFb3fRl7QAwUgIvEl%2Brth4YqyfCSCgZfgyxwWmF4S9Itdj9cYb4GbC8f31W6CRnTQuRl%2B3M6iv550D%2FTF%2FTGY8peH63cxvEPQA5FztOcO1Kyk8uZCXMb%2B7cdjkbAEyPQeuSvy0N7g5Dz9xkDm%2F0NZL6H5K0200mjqpfEgEh06A%2Bf6YwkxbjMN62AWPsaEulRBxEBAsnd807g9F%2FsKFl7lZ0ccdYJ0YJzr8Zm%2FtT1mUHRd%2BNR1v2qlGxoR5IeRYtGarVSQagzAFUw8%2FyDa7ZWOq6rkZELQIYUv2A%2B%2FVk2zzEgXuy9v8PbPLbyLEx8WJjOSTDq477IBjqkATqq2eteBGMSGenmQUj%2Fh8yblU3N5CpVlePSIkQ0pTw1OHLcW%2FpHfsTlVv1ldFie9zCSPgmovCMCYYI7O8ES%2B8ncnONlgR02fJnA20l00A%2BF7TzwmdYlyeCBFcXT0SOSLqkH7X%2FBuUaKIJqjVVLGTt9Jm8RAD39L%2BKRHO80HB9fVcpCShv%2BLWfwDFltsQaUlAKd4qsZHBjLxQk2r%2Fgynx7c8I2qF&X-Amz-Signature=12279ef3e358632f06454830ee6333ba12d867920b39d31af8b4748e2073a536&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

