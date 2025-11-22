---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VIUVDUI6%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC%2Fal4tkOaOzUllmjPH1mG9ASzb5Upow7lL4gaD7xOwDAIgLNrgnLD72Fs7FJzDzbvoNhaAz0Tv%2BqcFJFaVsqgkbL4q%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDN5GyRqMAu0fhxX3AircAzsSl4bsxrfbXyuGBdhqrHVIwQb%2Bn%2BkF8mQGP%2FK%2Fd4Ne5TQ97hM3bJNFyh0GqmtH57ObTq4fPvogJ6JEgY8DXkTkltV3%2BDScHNJoJRkgGoUSMTGGDc%2F1WUH09fZnnh7YkwWQ0Qj199L8F1669zUnwWd3YhDqknDLLy8tjKg3fjevw%2BXw7N%2FflsGmiz7bt1dgV99UtCA29GfNRdVosPYsCbcDFzvDAiZFHbTH33rwne%2FqhqpVymZ%2FjPToPReps%2FTDp%2B1JP6Qfngc97Dlz33ypY%2B4dzbDaicjQbcsDlJNrAFhGbk6vQHdd6TrDyLStR%2F4ehCXJVvLGCVVUGF%2FAQVwcKB75HITn%2FfmaVPf%2Fd6mrZ%2FfAoqSzAx%2Bz%2BSVLLPJx0GXgWpAiGsU%2FZW%2Bni1V6H5ezTieS5yhucSkY%2F5BRP4MsMbl4qdth%2F6bGDnQ0SboHeLMsUtvGG9L4rtMNklHDMBZQJqnOnAdv0vjbfiCx%2FyQg%2FJzUNGOepqYYbVqv1SKVgyB4FrObIE4PRUNAEUrxmPukP17APn5mytozdBGWqE9MyJzwWhcRBwptTWBy3RZvX6GvgvMXiAVYtEP0EU8nO9mM0ihnjhvqkrwnQCBWlQFspnus20H%2FmTf4Cwo0eLipML%2FFiMkGOqUBXAmr3CZN3oHn6%2B8EmMnHbpt4BXlbQt%2BpkC9bV5f3IcEQMMScgBBCSIltdtlYPd26NNmxN49gi5TCLmfUUoqvpmtHqLVq9Ss9M6dhZKVZwouAxyBRoEGaDkE3r6PIkUBUjje2AxyT46%2BdsfBOdSBEE1Rt05TOGFbmkQx%2B0XqXsh16px1hDK46BWTss6B%2FYred05S2hGp0NzbAy0Cup0Qx773CjSEU&X-Amz-Signature=e75a61c591baf009da1021884819310f85098edea406712e8d8242777d7f6d12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

