---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2Q3E57D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWn2mcYNNYeF8HPJBvgpcCgcMBGTz4BFo6Qha8uSJrLAIgAYCJBMz03vDjkPN8H9rR6foXOU%2BbgyvPibYPO5z3j%2Fcq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMTti5Q6IoZNnxTy9CrcA%2Fxk2PsSqqYkgk1gilV71JSiLAAoeN2i9UcJk7XT81z5ZjbsOAXfWEwYkK2ZLTyM%2F14irw10IzOVQIJSPG8g45zxfkewDQ2wHzKkWIIRnXVWZWqldL%2FduisOj8zOCrBsr4Q8d6erCubYacUJnFE77ujMgjyhwRD1lKYLjhrDOnpiU8rYnf0i4NZTMI0v37L1V9JLYVi6oYf%2F7PE%2B%2Fc5L4ODsyThs%2Fyw%2BzXZun8mq7htkauNAZKGPJTwDBP2l7wGfrO1y6b7SAk%2F5toONUbh031%2F9cCwtCmM7kAqLSMqmkRhF6CLVhHN%2BboZZn5%2FHykTzTFmwgMSTZbupF%2F39YKBlvYW3NSU36hWtHFdMFrpXNJvAHMMz4CQVjDtWnhA4u%2Bkgr2S%2Fsn3ryi4nDXmTGk22GmWT1FOQg9tkWnneaYIflwUEk7m0MUYgs3DPPmZ4mRlj0pYUr%2Fe0M6gI5rTKyf50dhXHdE1%2B%2F%2FM%2FfO5LKXvnLJLXZgedZAYeKe7chfjBvwvMaQ8etstFKlXCE0jF7kKL2J4eyskKOUBFtPqR1xS7MbbUig%2B7MXdCKQP1sGFSg7xf5PImEMZGKJ5et%2F4GKhlyXAToLo57ocyA6DNhatD7x5Fn%2BsZ7QPO07FXF74SyMOyvp8gGOqUBGAQ09aZoRVgja%2FAeP4YdmMRJmlwysm1ntCyXATrAqxzzlvFEkzqu%2FleqCyOJI7M4Ur2Mwka9yXo9Gy2WZiE4hRROc9jTIXKiRU35T2TqALCtQJNO4%2Bhg1qQr9nwE5uk6DCiVIeMVlN%2FSLBrwrupJhMGADYpGP4bSoXKxrBXq0sHfdo3DlLkQoh%2FhHMiDHengr%2F1dKAppEgzWmxdvYV%2BY9MGjHEA4&X-Amz-Signature=1f44facf2691de1c7daead28bd9b7b46c1debc8e3ae1f45add75943d2bdbcac1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

