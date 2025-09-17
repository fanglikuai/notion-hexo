---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665AQKFEX4%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIFgPD25r%2FO3Fp21qH%2FcsMx1N0SYQOaAIHwVx5JBEMyrNAiEA84o0ZeLhEW%2BYDpMekceXCkWfZgNv1fuGZIq9szyTkesqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMRzFvTKZS33YvoILyrcAwKP5lpYtd33ffZJsGCMY9njg8kK9FVDlmMFVMWezJVlS6zrYve9bYJQmH6RlkuXgjiLKXI00ad2VE3oTYpUMJFwvkBRYiwiALnKxX%2F0qs3FEw%2Fe2QbtvLs%2Fjnvr9OygDzsqCRabYzPX4OMEGtaV9s6itZsksqRYxBLhyGAd0EZ7Ijjnky2khRX4E6zNNe%2BjTWbNI5ovCdqJAPms%2FeaYRCa5JfohG8O2fDadDsB5gHwbKTqia1H%2BuAQRzyHrx8ZMg0U9CEuKhI0VyCYwYDObhpD2JapLLjbHQYOxpfOrq7%2BXm1Py1KVz3QkjkVzTAL91m%2FpE3vIW%2FmSC7cgP9zNuKSk3aanIWBaMvGmweJkSauHhfahNLo0%2FkwoNLs0UPrLz5T0ZR5un0T4O8bkXnkRalRv0y92lgftBR6dkJ3ePts%2B6hhYf9MRnKCjmfvXj5xDlYA19mAMlZyx48yFJqLCV%2BUkUnCfw%2FGFP90o30nGJwdWLloyaL1snOHlD%2FjDd0DRnSs9Q0aJGh8N9XMGvQ0Jn3BQ5uiyAFlYg8NZtgAQFggMJPLXsOexiwExmvQ4nAkem%2BxS3KmCuZU8tsqZFLx2rXFmvWpvTEcwkdyfWeY7a%2F%2F0ZnArKGabzx1bWFn74MPLbp8YGOqUBR4MrzON4wwuDDQ4KGXUSIKPpIcSHDqLi%2FNrEpV4nNdkCzKFc%2BtldySJZb3qqXsKqXvaEW6s%2Ba5HeJN1rqg77AifOmXfJfyCbowCbOqckpDVd%2B97Pnz%2FLHDP5CRAxZxy4rSwEvd5cebiqyZRaGk63xR2JQSzPIO74dOHC%2BLGVLVpcYFvsO%2B8manfePSn0hveObGquOFnO61FoDR4upNHRwpwweSWT&X-Amz-Signature=f8b539cb6022309b55844f36d99eeed369fe95bf726c3b43c32923ef72147dc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

