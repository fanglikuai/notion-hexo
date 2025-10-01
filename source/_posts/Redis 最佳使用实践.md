---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664YAN3FS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T040127Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJGMEQCIBEGnFOY2ha4GKgah29dAROmsDsiwt5p4Eht5WhQK24AAiA3vO%2F3FgoxcoVBhirOscBSshoUf8l5KyBMyVWJjrBAsyqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM410HP9vWAwgxYT2fKtwDTkt45dPOWh%2FzTCtq3y3OoMkwOjcl%2FX%2B3FSmJq6MYkdUydWkPUsTDmo1EVrWN89VPE0o7VntooF5yGLLBMviyxkh06m23iDmWaNIxX4pc46xcYTOez6U9QxxbwFoqu8gEFZcD2AU5JNEs9HUNz0JrS5Wu8JsqTROFlLAkABxIiesc%2B8bow4wq390Wuf1X6FLwMtXPxWdyI6jrRfWyWbsMFvV0Py%2FcOVYGkSf1lrCXjXEUT1dwPJzEbat9TQ1jIf0zvzEKIAmR2S3sl9oi3wK1wXRPPc6T42F7LeKVwITcmLyx6x9HX9Z8U4MxcODJ3PKw1PUtm1HXEm0vg7K0buw%2FBlbqfGN3O5aeBdRxjcWZ7oX%2BJ%2FebXKMYnMH8lXpV9nkYoDp6hSjjVHhqLGngldwg1KY25q2%2B6Nblem358OOTDqETQfnGpUMFQIHiLN2nvcYVm17y%2BmWnKAMaDmUByN6bvn8X0ibky9HaP%2Fi0fNtRH8jXcsGg9kLM7%2BHS%2BbXuSTXkyfjI7gVwfmIA6HlmcVBm8M%2FsOWEen6PlgaL6NxQgQr07wpyJ%2F%2BCBn5IZQMoFsDHaNRbcqLw08jK2UN1vvTuorMh%2FAnFWcOhDqfGsRUCnRNoMv4kA9so8lW73GkMwuc7yxgY6pgHhZHUTuTgBfkIBTdEsyrBk7VZ7a5CYCi0KxankWgSn38WCawlsy7yJXpiHnCN1cr9r7xbOJccFV7h1q6oNFRMlPShAv4rz3Z3Gk%2FRIwIawpLN%2FbgiulDdqvqiRidQ%2FXNnPXaFpYEr0aF2oxAiQQCjOp1Bg6tjQovIcUB0Ka1azOQsCW7oRZXehvj2zCpE1HdpAKHGRGhhS%2FWUaqMIQTjMgE0dA8s1B&X-Amz-Signature=3a8b7ad182fb5a45ffc752bb3c6c4ed5e91ffe4fa3bdb0a6a4a81e19e15e1202&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

