---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KJU7KDP%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCICURU%2BtS7MA6c3ki7eFwWWs9EHa9jQnv8hUZE3a7jsqsAiBmNMhfe2s%2BuANCvUH2dz4LPHiu1dmjg1ASJ4kCmsyhxyqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhwW4LoZTQIHYFwklKtwDvWstsU3G67zFA3q%2Bh7TZGQmyqsocg2oiT9PzSuK2ynOLI8cVgyJKQO%2BOksDFlSdfbnLBLzak8uhxdAu%2FVTCiTbw5ZkXfo9hLJ8PuPjkiPmKRrjhUo%2Bjc8p1QZjmyBT6f1Il6eCLyEMbuP9LCG%2BdMCXxTDKl9uqBlaaTZaVGN1IIlq9HGec5G4hlkYkDE116CjI2kVlIY60l5te64B%2Bk36IBPsXRNvC0g9jwTCAHWldmf%2BKC8tyq9FA89JFb%2FAlhimaV43%2FbtT1misdzOI7ePIeT5R1blP0QaPg%2FBRFttcgZ4j%2FtFUeH%2FX%2Bw0UmyAK95qnZvswqFZHT8DpdYHPkSph4yeQRxPDsuFdVMJx2BqU4zSK70iCrlUYS4RaZbn%2FNP4TW8WraXTaza5Ds7v%2Fq%2BiTO4CCtallRuOBaVPrhor4qiyCdL7SIwDS594gRLGCN0Y7%2FSDUKHTyCLjrUXyWuuwtJeQJk574tlT%2BtxIHFoK%2FozptsL4NqkC8QbdVXp8zjPGvFqwKP67wT%2Fwn7cu9Ay7TM0tn9aEL1jWyyGWMBvBirZM7GTDdKZIm9l2NBkJb023RSm2yH0HXnFeuKhToP7Speom6k2YNeiVD3fQY4OJHdY%2BFpH9ghZYK%2FgOC%2Bow%2FeWjxwY6pgGDCBuPM1RyNE0Co58zgJiT771rDYzXKSqAv7mVgUrJ9YzFvFE1CjecDQGpH4Sb4un8oWdguF8b08ylaqFzbmtJb1S97SDlnI3yKVD59C6uzgS47L4mra%2BgT0Ase4hOhVytJq8XeP3blYj%2Bz1qwGUmfNn7OfGpwU8UYiNKPY9qsno9%2F3F9jyMBaLe1z%2FZ6wuZqERWkcwCjlKEMX3THYbmq2p6%2F74O4G&X-Amz-Signature=c9c7a0619f4e02c6b56edfb3ac04d4964a35a9e8b4237e28d9315dfe8aae0158&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

