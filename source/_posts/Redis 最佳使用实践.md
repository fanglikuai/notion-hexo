---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T24IOI3G%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDqWRH9U1%2FUGObmpeu1lA%2FusuQcWVZcm2PymXYjCUTvGgIhANCt4qXaFQRF9HOPb9ELwZQyuBPu6EKrQBDIZtvOdjGeKv8DCG4QABoMNjM3NDIzMTgzODA1Igy3BBPzNtxLY53C7MEq3ANQpVbEzRI2JKeT6803RewN9LxTn8eKZGO9iwy3xBUrqpVqlaLR0K%2FGh6EtuNStUjgAidLZ%2BdGPzfUGF2ClDth%2Ff9mMOXBcQH3XeTqPjzY3zIqBGH%2B2aYstUVrCd2DQTzqBmUKO7U8WYzcJKwhIb9YvxwI2mI4NW9HOplskzbGBqGrKB%2BhZMp1J8RQP%2BPSmql1XCMPEjgTIvAFM9MOXwkGhpNVDu7orWXBEkBLuQ6GnjVPkTUO8KUbAJZljBLUzq8090j%2FEORxPb5hS6VJtDbubnhpy0IsEQ9KcvmHRYP%2BH%2F9HuFbOD2AQ2Em5Kn1xlRmVrQn1K8%2Bc%2FxdFTaF1b%2B081Av0cytEmAW2%2BTAQZ9PWIA9yjTYfwI6zBEFMHUejdwpHugcK0eCwjvJzhGmRAvTXYqKVvD3bwa5Jy4c7Af37cRjGi5tsLpsTDYOXbTiYXpcDcEGY9iTPS1DYArPjPVd8V%2BmHMqSZ4o%2BdVSYEOBbyXwXFUiF3v5fFbXOn1Y7hknXrHg1U%2BLIIQUuuPzm4c0e4ZR0VjDo4ds8sNbjMZeCbAPRVvPE7n5vnoiWT8DP6AUOEuXX6xfotybe00ZMlxO7SAi8DYTx645LZwyRPECTcQWvSPN2BaZdUODVEhxzDOtd7IBjqkAWxPVzQJeYgavJ1qPytbAwXFCw8k3FYi0uSN8ij0%2FesiWy4LFhuQW4CoDUCOPMJSyQM2tyVGKFr0IAy9EJIHYGRihSjoAuH18XUau5t1Ha2V130BibpMcgvsB1XSqfRcflqtHc1tY7b7HQBsCLthknCRePt9W8GT9JBhLdyyXwiaETuYSlAOMJ8L7hbo6pk5pMb37H68xKqNCf2LpjVcoP41XXuY&X-Amz-Signature=ec29ac56d498cbed5e3bb68ac1613bcbc6db77309f5577d733faa5ab03a87582&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

