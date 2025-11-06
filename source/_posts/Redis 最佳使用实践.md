---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFZJJXD2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFWXza5ndkioXJ4DG7l2v5XSB4Tr%2FAzQDPWzkZPgi1cQIhANnEPKCANEv0UtJ6fbM4t4StOtUrZkM1wlKfD99BWy1DKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx5igE7wqLrr8I4JMUq3AOFGRpXua%2Bn%2FqzwSLjmBujwAEix%2FWC3MAeBhUNOHw5n1riiVzqGlhxoRNIn7nqfCaNDkylzlz9KC8h7CRFk%2F3%2Fuqzh7yM9A%2FQ1%2BbjYrftIoduYeV%2FYsWQJZW5e09Id%2BEMN6Nf0%2F7gieICKEdFWOYJkzry0oXkYRgzVgDmhkEEDuNNcFDAeAYZ0OT1wcTb%2BK%2FQeOiHp3vMVs9KfWVqXCdaD2iwfHfcosMMhSmwxJ%2F2aag5Z1isVTxxNI2StlEN8HxUzAMbtrFNsLNb5N8CXKadyI%2Fjb%2BinsEyZJkItuB0C6uSekyzI56C3KrZuK0eHGnNWiDsVWBE%2BVj%2FmWVU6%2BR96IX%2Fk8pIuEA7QtG8hFHAr1pdUJhWgiASTHJ19vcHT3LQtjFtQLy3n40n2zVn6DPyhUOCAfd4W6wq67ty%2Fr7aF9mxD8WzY8Xmd4vUAmYnJG6%2FyHP8PvlMh0F4fQnKXjwPf6A%2BN7FO%2BIfZW8iHHP1V5sFe2cOv8EzeCSqatTLTqK0oFbf9pED%2Fi64L6SV4KeD4qZL3wdqNmgF3a6%2B4mbSxrgDnhkvYgfMEQwX5PWYMrzJZjJ%2FdNfURRNGNOlVrsXiHlMqAbtjTkOIm1YQmF5sEJhdx26d4u8OPgturlUgADCTwrHIBjqkAe0M8BKxJElJ%2BFkhT8DNcQPip1KlDLSy41ru%2Fnc1yb2fOZEZKtKtOYxOZRq9zA8OEBhBqpO1yWqVh79SeAwBwzTFAfIjxpLbsVALiQ1%2F4881g3gzzrQK5zjykQt7XCKUxSrrbdMkhF5h0%2FztROA5UeUmw%2B9JlX6nwX80DiGyMuWrWW8xDJxt9Dl%2B%2Bn0Dc3K4idK2Q27XqFKN5dSGcA8Xo6N%2BMMej&X-Amz-Signature=afa16be1afd74f89c41aae8d141397844a7e9b284a75d7793837fff0ea4adf68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

