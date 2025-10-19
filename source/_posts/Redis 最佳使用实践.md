---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DVWLUJJ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIFMwbVJzZ2mV4hM90SglyQre6CUxlhpMlkUvEo%2B7gH9MAiAN8dF%2Bv6UwRyjVnoGeL4593I2AYJY7Ksi%2BBvuyOwE%2BgyqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWTf4vfLNUNPqSmdkKtwDFgjG2iLPi%2Bj%2FgX2ltDYUkTE%2BCWRjmlg46nfVmQtOQSJOtKSKdJTTYxU883xCD%2Bg%2BaSJO4jBjEhFbNMqkqirZ4qRrAiZRWLNgi6Em0IJvsoQ%2BnsEclndARJOeEvJXcqLRTwEPrR%2B2VDCmRjJLg5%2FDVz58oub2YN9VOdjstmVzoVRriKt9K8EY21NtLKzHCKfd7%2Bk%2FX1LwVdXnCWKi7jzyer3DWEC%2BMUjZomtCtN9NGTqIhEOg1kdpKZ1nS5X9opESFkNwOscchOOelVL0i2YHyeeRQROT5yz%2FjnS4PhNhCcPPfSSuCZS3lIfXmvkaWoXlVQDfaEzKS6Fn47xdwhUdECJvCvDaAd4a%2BR2h63HXAjYJPPMzRdCXDIlMS2AGP7tJYGeveyeXbVaNONAhMqLvR12RQPRwxf7NZWx7FfrhuRPO45IcCSBPKLeaCISHUU4RTbmoQeaS0d%2BPYzFk0UQ2eslQA1hqr9bxXG11SwqH3dwCKM%2FLVetq02q5suL2WctaQFSvMw9Iloj4%2BisIb18BjhJjnLiXe%2BaW7oUmYcN3HP1lOgE8dl99P6uBsa6qHq4gSqDcdgYHBOy5tm8FnuW5Yl9AegdNeTwH51nLaH%2FvAsmYE0Pj%2BPFpjZef844wr4XSxwY6pgH3B3cZPJKNDMl5qM2xQ%2FBPzHS3RpYR%2F7BjaY80fNmA%2B2Xg7KO0jLGu%2FnuH7gmRHusIPc4nf8q1Jcxc3IcKezVHGAn3JgCbkFo2gyhDa%2Fh8P46QlxDso%2FPEazuhNLAtvxAt%2BV1fRKiF2b1aMhZZedC1FgukqKO54O7cOWEabCqwQHhWmXyKAJwL7pdxSYXwvba474ZELe6uw0v7%2BOu4bpKSr5qUbjQk&X-Amz-Signature=d4c6f11b206babfd16f6f25ce9027743801489ef2f901c69ba58804302664540&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

