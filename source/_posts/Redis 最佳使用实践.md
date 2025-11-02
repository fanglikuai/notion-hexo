---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTCJ7YN4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDFXc7STralWc4tOyf9i1i1GPcXi%2F1Sb1pKY9JgNLhQigIhAKfyksRoK7Y1Lefa%2FFP0i%2FJ5jLFunpAWqcQo7vMg7EqrKv8DCEQQABoMNjM3NDIzMTgzODA1Igy1tE54%2FcFRVzNp%2F74q3ANE7uiCOpRvnEFaBh18kFR5WTFVaTLNFsngXlxyVwynnJpOfEzqY92HTuUoJHEA%2BRd0lD9uCIqrjWj9cfFvjl4AtBjD6OaPPTLwKeYUBCnwptM5eo4c0WwNaflFaMgRpBir7YANezSHyL3%2BCujzKmgQya964%2B3Ly5aMPchuZXD8PfCdDVmaSW8%2BOAeHzTvDkFGxwxkCCUxXWpVDnorySW1avB8oqRsVQvHOPNlYlfkEI%2BYB9l7CNZ3CIKTB9rVGguwvQ2aVVrQoNaSA4xXqiMote47al1vwOeQ2ol3%2FMu6oDrRrSxAf92jwdmkls9i1OZE1M9%2BDXRtU0coZ%2F8mg8FEfqMC94%2FJiqEtvtZPIDBlmtMyX4oIg0pX7ZfhpCH1lBNjWtqYgYGeYZbhsUwMetKoJrLewkAF%2BoLr8jVDJKgr2Ml4CHqrRNLuBeuUztieeKcO2D6KmsvwauWvgptDooMzxUYP9C3hTAelvCldFsFNlt0a1tFwSeGIJpwGCUy7HwWhaA23EOrXSwt0GOxYOFKZIgUAV7zzw%2Bu4FSMBKVrx%2BzaA03Qq2BSDR1BUj%2B%2Bf9W%2FntAIa98D7Z9zZtQTAe1mMh%2FxJ7Juu0CpKDU16GkZTXqAkL00YUySt%2Bbw3EvzCI%2BZzIBjqkAefd5Gjowem1Lf%2Bcxl%2FS%2Bx%2BtpXrHlo5kR8ITvbb0ahzKWiO1%2Foep2FYE4vfbMxx%2BWm%2FbxBIrO1TQJiezeQHWy5S81HZ2NKUPXJjVmGReRUofYZhhjIlKe%2BHIsiFDihqK3MLYfI3o8ZWLPYfXIfRRnGeFU6%2Bttlyx2e6ArgwOoZENj2UFlR1UUuHVewhN7rXntzjQ9dhHxdooEZki%2FFZDF5VRnqLx&X-Amz-Signature=a0b96251ce4b087b0ef818b8d9e5381cf634a5925b54d8356b8a73f4ca54b4da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

