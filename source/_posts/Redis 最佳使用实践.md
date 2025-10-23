---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UFVQNHS%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2F659yUFvdJyjXsXj3wnO0WMkaf2rrRLS7j32bKUxGqgIhAJiEw3u4fCsnPTQStyYupIxrOyVxk%2BXrx6wsOXGl3X20Kv8DCEgQABoMNjM3NDIzMTgzODA1IgxLzSwJSIYvz39sjukq3AM3hG3WcnK8v9Ib%2FImsOiGVANPdhI%2B6ecF6AqaUS1vAYMAzjf%2Bnu7E%2F%2FnskrJd4gCDzUhmeY8vCHOHYVITVtAKja%2Fq%2FpYLsP2pccrYwj6xj%2F55YmSUpIaa42wAMMnOW3lsjYT399N64FZ9oaaxULbbEZ2rcH6rj7UjZGRbl4YgWlhczQgN8I5gYAYqckyWAdccts18GJybyRSCZcoYWctJo3i5S4VR%2BSMdpCDLVXSYbJP8oWVykZh8C%2Bu1NkQgjYgXzjxyxwHdbBq8Y%2Bm2uqB68dSWeFD5BCtFSDNrahYV6isvtb%2BGXxIue898gFDXj2ZsujfOeU0QEQVknpShNzTNK0hlOmi5ERgy0aPMRIQNG9is45nloJ%2BuOmmXmkOIgvkWcyuV1Y2SaSmm7p6qfKJP0op4mogIXO4aBQDZE99WAMmKA0sNnqRDpIYoRPTBZQuL5h3gOMd0Kur4XUH44TDv3QKRCpGtppbbEgUkdNhgubvL7Hun4CTF%2FAtryQGK3BQsDQB2DKQchjouvoBfuDUyjpySaEwkjXNMML2bK3%2FVo3Nt2QhoYuvjH%2Ficeq1ud7riLATMZf1KWzKKdWmQDMd5BOfsjx%2F8PCgEnFe5Tid%2F7uNeDzZDAgzSDSYb3QDDs%2BujHBjqkAUezjWVoG58R%2FHxhnnk6vXd4Mn2X8EGBTB3LWzl3KZky4tio%2FiDiLLTIKKAWB3krs590M4JRt31kQTsp6n4FwB7VT1ZwpfYPU3pGPTHDcNwtsGscHlkF%2FuGMsD3fkK48CRN3NHm00ZFJ3qiYQ%2F3NYEbZoX9b%2FIwXK6XqUF19cKyqMvgErC9ll78xnKMd%2BGJbc2%2F8sWLSLdF8OnU9%2BaQjwXbAdueo&X-Amz-Signature=3cc6b44267c46efcfe933c7da7712cb438d0f6bac605e5f6e804abf98e8e45ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

