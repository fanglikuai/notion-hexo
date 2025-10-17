---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCQOOTKV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BQL3vvAtjlAQDdWEVTxfFKJ%2BeZ3%2FD2s1s4vA7I9CKKAiBttWJ2PPNd%2B3hMYMWKeH5NCnV88FdDhyDOcbDCPXbkPyqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjnATRKk94dUgn%2FRyKtwD%2F91HQK9JDIPOnTDAyuVRfl%2B%2BRhG0TFz3L%2FlaJfLEfSIuuz4ny6VFQnujCmehQkn2Evej%2FU7SxVf4yK1J4HPE7ZQ9e9%2B21zqS6ULIeW6hEA4CBpMUtxLZvG%2BrH6Zb7N5FzPqW8kwtwQsfGIfohi8YnRWddMiICZleTNl6V9eUt50AW7cC8MhxhajlwRLk%2B%2BgeX17FtbX1CVk3zvA%2BHwVMjNLFuJe%2FsopS03LEJt7t5SBUorbNI3SVeV7D99Zg9hagruZpbeu4cM7Psk8tm8I676L4ahli4o2kojJwQSShp%2Fv3o2%2FHrsA9JMKifB2eGg5EitEnHpgXFrZa1JEiC7Fevpv16raQS2%2FGcVwYX2pyxeZRlFuDBzixHgG1L166uO8K1Z3YINvSAWCXREOBOkekVtbBAE16he1lDvz0kZ%2BE47gt5OVy0QLHI58nQQy5dc9npbZCGM%2FPasLkmuMz8N46nChPnZKViNS6qBVMNwaufwv7n5CT%2B1bYX9lRN4uDbhwjayn4ytLzKtY5sW9hLBvAMe%2Blo5dO0RvGdyS3P60mzWPSXh4ZLp4rhRY7iLCOHmx%2FAGmkM3qTTg165lTIG1fM8uZKqpg%2BsNDscxIuQtHrf1aF8EHVoNiCXaO%2Bm%2FQwgv3IxwY6pgHXas08hAUq2zorG9AcXiPoSeYc3L6Ys1uPOQroFDxLi%2F0eSkhDKKCSTCAKuh3pnzVanlyH%2FRKfu6GvwgghOS8iCEEU%2BJHUi9b%2BfTNiBectboM5PBPc4HLCysot7rLr93i36Jol4gwe%2Bd%2Bd9WYfz14Flrd%2FpMJZbubLb4%2BFvjmAKLrDIFm9J%2B0%2F6TiQ6hnEioy8TDUun3g42XSk6CE8NlUVm7Tu1YWZ&X-Amz-Signature=240d05515781260cb59d7df3612c81f588b8df2edd7421764ea2643b6174f668&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

