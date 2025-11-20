---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665K7ZX33L%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIGA6XeLhuoMc3W0c4dXWUemqndsxlnvhZTb9XBYJTb1AAiABIvkZli4KPhOFYYmjkAEfeNR9VbxQLG8jAb%2FopuqbjiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6nUxSTL%2BDS3vPA6mKtwDK7pYToGfYyLLfqZSG8z9gt4p2LFp41HyH7yk2e8UCsOBawgOiUv45ECwQjgmY%2FeB7FoyPmcS4nKwWC%2Fo95nYBQ81topsZFjDTrMmVvXHAjGgEnDooVGL%2FxJDtXaAwsyTGGW68JnNVbLb%2BMiIxb90fkuPMhgdImk6Edou%2BU58Wo7IeKxv7mPXiZ3AxLagJlPiUEQwq49c3fEVTsFH3XM%2B5ZOZt%2B7cOClwr8JxZwEgkiwM94IgJbNFQ0SrMtI0ASFRU1hbqtcO9TtXA77M9gj2o%2FumKSVXorLfcueqd9cDVeRSE1qOSz4LvKP1Z5Nto4N8Zmna3IcQD6ZlweRRxgGeEo5PhEM0h5T0SK7o4ewxNTlqVPJp24wYnHaKkFFdgiUA4dOUsKob%2B1WI54pdjIHVwMY3JWHuChX2XxfC2iDPmcU0cvxDc6vJulGtE6LM8uRvXtahxjsZFd8SCvO5xqwiutZs79TReEp3GuDCAmr%2B5OwWnnWWPWXJMW5ubzcVC3%2BGjgSHOAlpeG6C55z8Cyzv1B4xxLxJnmmVGy5oDZGAlMoEQaGNrUqQ9NcZSSZV9klYJoHN6vaRu5jSJmmLaf5iRKbjyiMCH2ECsibTwNL%2FE5HB4Zj3p3jbuEnjt8kwkJn5yAY6pgEHm0O7rcbwAmWbPDDSINojgixDyr%2FWVxMsrbZOCNoSN6y9ONEYmewRPB1jUfBiPGpMCVf2uF1UdXsd2A45Xw8sA1Z6CsRNznEQ6S3oLXr9%2FUhT0xwb%2BM2YJL4LX465KAN5QPp%2BUi6%2B76l%2FaLqJo9T9PpCBXof7iaYLZ2iQknQG7OuHZplhQ6n0UFvbB%2BaHpYGn630lz%2Bhvlc8s4VhEBNDbOMgDIghn&X-Amz-Signature=012be47859f03c67f0956bd9d3eb93f55200c34caa41702a9e6d64f6e35a9f3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

