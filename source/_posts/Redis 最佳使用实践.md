---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NQX5CTU%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4QBcFQNj6bMqcG8paREi4kWmt%2FgjSgNa6EQjNx8cK2AIhAKHkZJ6uQfo0rqHPx5dNAfjsuhDb%2BCc2EQfHKI8uVuKvKv8DCD8QABoMNjM3NDIzMTgzODA1IgzQhFyF8D7Pz9Bnl8Eq3ANwS7heo6%2FHtNpVnYtUjZLrthCJ60TVoQ3ZjQBRCnm8O%2FDs3PJxEhx4vTNiqyDlR3Pv9T40EP5gWg%2F9uU%2BU8Q5JZ0%2BNIm2RCbNQ4Plkg0OnWuxg4cbe5gDXBSEHKgjinBmXOjB%2FqWe0%2BBSOCw4i%2Bqem5Fu%2BWcDpUevO2f4NEYGzWL%2BVYfDtAc%2FOIY2DaNhGg4Upb5XXtEqlDfmYf35i57kmHdMuNAfU%2B5PtYZKdeP%2BMuilzVh6RzSNbigsiM69jYvooxldN4Z03xfcJZiuFXiRx8d9sA7NVQGaoR8XtLG2omaHd0yiOJZ9oVfqNMBsNUlEIipRpj3025Oi27eHzva6i01rYhLIxncpdgTJivYIcBT%2F8PFKKNjmPuVdPIePxPuoAM8ztzyDbXMD5F5Mp%2BRLqDlgPJFEKjtnsk3aVO8e7RthGUUiXbGNd1pLdvTl4QyiV9DinF7%2FmPIMndmkfdtonfPLPsPXGC0p%2FxqaG7lag4lWkqobm2ssvyrndN3pSyhW15ACKo87NpUumUsC%2BBb%2FaYHoGae2GPet2oIAg6Opo88k5YiqLjCoysPXAE2wZI1rbUawhLoSPQ9tH8b4PCBNGRQS%2FkzgdrivDbcEbkqpNMeL%2F0Q6bSilDJZGvDzCu7MjGBjqkAb8hpCoCOnzsucnKv1yt9YjttmgEsgiDFeFGatSp2Qq7G0uQXa9TNZdrUZ99%2BXz8mJEjKTnFJ4k1YysEStI41JuFxBkdbsmz17x7JCroBq4nNRu04nHzP6bHOTr34ISUv5bn1J%2FJDZgE4jzsBL6I2j6%2BkEl3GKV9WaYKChdY6t7yE1fxwBqgSLWEcm7x3bruY1yIjIjzIEnf%2B5o0MMHWbszG3vxv&X-Amz-Signature=6f129931e4dc6529dce5cb14f7580fbc0feb04bc0fc280d5e9d086a88a7c65b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

