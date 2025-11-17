---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDYADMJQ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC4TJ7hoDzROwITxw68pUuaTv1lHuGVRXLE6Ga4uWZGTQIgPFjj6cT4mfb8VQkVaD9m6RIKGTki8dSkqr6XUde3IkcqiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJLVpwKdf5VkFQj0XCrcA1BDFkpePLRD6y01Iwy8lhpIB9pqkHXWu5OqPcvXOLEGeH6cHqJVJAU0elv%2BXWEyrdIHjYa1x0ppodYEUrM6YKERUWFi7sGhHDestA36xGLAR6SMmXMqgiJmZBLUU71bjI6Y9MNVDUUwxvgmgqnLw907q4Fy9nn%2B0mciPQIIaljArEb00jznCNkewZZ14mWE5Q9ambjTgxGCrB3DDIVyyqCwCzPdhAPggklECW0Z1yyZ4ZFjBqLlRS4sS%2F6KClmFH2XwKQWYUZ0nSloau2JBQ30le2h7km0NMpejzH756V7T9jxcBMubvJIMyOTzwD3J%2Bjf%2BhgwU1ThEkZ5GChIQN8hW3HwDZg7Nee5qKMbs85KjiIRvVtRFnXyqWSpilclV6rxN0SpUQsiSeRL0tI6bxiG4Ky2Q65f69VkJJf1272dTGBwjKAF76g709GfpaRXUBk0qAFucJ0mk%2FGETQClVqtL9kofAtUOelUDWDz1fn39kiC7zZGxxCnKt7%2FyLn0%2BZAR1cvQre%2B1ENcbkJaYDnGUEf1G8%2BHU5qpj3OUCL%2FlLVYrpUrVIiLrmRubSUyesIYndGjfDadyHp3zn5VffG2AMy5qP4S59BKWILoiUOXUNQ9ofBsKL9bmAT6bOITMIiw6cgGOqUBuFoA7ypyfsQmzLRYfOkjy3Uy4qgw269iBZqWa3jBFio5aM87kysiH3AoXX7Bg3jzeAJo9LxtXRcPHhZFJLAPa0tHp31RCN4XPc5XQGxiwnSA2TIeLaZSBweYCdlz8ZcXJpVyw%2Bq3LVlVgc6urGMX4yvEJtRfJXuibV2T8R5x300tTtZIi%2BlC6ZdWIuyRqDzeMKTctLiPy2am3yG1VpQPJcUCE9zX&X-Amz-Signature=e464870b5bf72a598999bf6664b030aee8197f309528f84aad51349b247559da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

