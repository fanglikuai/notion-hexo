---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YMBBQTO%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDokX1KLwBIJrEFf3E0Zm3mw1P4XBkt5abX0xakiHGjtAiA%2BicDvvnAd0uiuuvcjkGDwHT6izK3aJpzvolv5we2Q9yr%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMdmWT1hoNevaWbf5BKtwDSTeei0f3nhrBmOv4j16EBIwJ%2B2Q3q%2FS0%2Fen6E46bPJdHmrxQqgzS7xZTkDizFYnuA4gnv06Bgf6aSntTqGxuC3VDeEXR%2BWOLqx7Qt%2BSYBvRU005r98eYXeC8LPRDTELoV8g96jUJLs88mgmkjS15VJkAf85bTBhMZuh7ODMlyvICgBR9XcPpOmoNGxIhMlao84u4kSwivQPl4nxTZGZcam5ndS2Fp8UhjgmPf5OZ%2FfgZNITta61lm0%2Bnvv4sPqdfhWEBm7HykN8%2F4WnK9uAoXQysph7I9q3KksCh0Eo9818FzXqKbcFnwzj4U6gGEjYY3OOQZpYZopi0ZCLwUZJIWTBHNAq3sniEneo0WY%2FY3YgdIrm0CT1Ur3u8EIMR5esY51syTBxujQ54Xmey9jaC1VvOYcwUYqwcTWHz8zAUtZ0cmxJLqHwBVomUdRg8%2B0LglNJuAOkipnMp%2FWAA9edcv1CBxMNBO%2FwZunhRPO04I663k5XQ%2BMdezZyplWuvA6d3QD73L8r4xHw%2F53WorjQUpSKw8vZf3QF95Wq67Qji9Dv6pclGGp%2B13s61qEUMDCP1R3drRJ3Xp%2BOKfFKHyTZQz1%2FIex72PNotWDQgY0CxnkI7pafN6P7aUVktrbIw1ZDdyAY6pgG7DhReogXs8tkmrvwarC%2Bm0hzH0Kr0Y6Z6%2FsvixXXal1Kr9hQL9O1fPZv5%2FhvnDK3CJ93jyWshGgeIuqGys6nNm4HXj1%2FxApaMrS902Lx%2B9NPyqDytxWNI95cCxtEGS2xlfv%2B%2F2V59bqOe5ofTns9Ck5b06KHaU9SS5GsgtQKiOduP%2BBJfqcldR0ohWJQZdGK%2Fc%2BqbYM4bs%2BOAkixRz8HocTobJQea&X-Amz-Signature=2997b33ea95734d4d5dc022332fb2cccb5b7a3228d89404ad6551e2a9f0ae90b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

