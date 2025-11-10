---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXCTNPSM%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIAe7rmbTa8R1dVqm7%2FUECZLDTna0z65fJNfih83gPI5KAiAazKKqgwQ59FCsN6NxNrCL4YEBvKrIyGx9%2FcezvjVpByr%2FAwgPEAAaDDYzNzQyMzE4MzgwNSIMb150dH7BArtxrGMJKtwDRnJDAd6fkeLNKIkMmdS%2B%2BkuwrK5%2BD%2BpciVQydHHtivkrXT3%2BN7xRgHFlw%2FLFIpZDKVb%2BZVOjf%2BjenGBb1GZqYdDINdqfS6OKwWyge1P%2FL7A8Ntf05nAsRl%2FbeD1sh3YfI34VEFbN0RX7iUY%2Fei5HTfIanCAM1K8VbeXIcgPCVxmh%2BqLb%2F3mjTdgSetkt9dLd5MplYLWkOeyPFsR1NOuOk6T6hmN62yZpO%2FZiS4v8dYWNxLSCljtV0Ixa3plfWJKxsl6DKOYoKEocozcqGIP0ocZc5e7%2BbZUpJOQXvZXP0vz2qVbP5d3MQlrX8rYRz1vZ5bcVbxREZY9tqbd%2FyrHpDX6OiG5ZFOMUyrn1ZkngvhCMFWxL08XCAxptv0XZzRChmuzY%2BMnXRoUpGkenzAp0%2Bd0egNTxtPuLpYaWsYWwTvy3NHG5CEzP80TuYn1BdLJay8MtrL%2FirAP0c8VAHdR918sKaycl1c%2B6KKhUDbx%2BD2FGlZLQny4bbYyon0mXUgArvuVft2sWKxvl2vuRqLCwvS7DFBwwd9zOg2596VGBPwPoHtZy%2FQJWZ3n2ODHifBh3WC%2BJMj1DLNIStgoBPQupYgqeypyKZQ%2FOwtwUdYB56acLxA1lj4orTld9f9IwscfJyAY6pgEcPXYJk9OIxuQAgp0zNZVOeogyECyuA8grntTV4AfGnFH5LCPizbycRuE2Bt0bkYB9jrWkUa5egbHJ5pAuyqKBM7Sy1eL8r4b3SCW5IuO649Vz7VDJPG0Ccx0CvpXvcTXL78rzWxoCpHTjN8VVml82G7uixWQLPf7G4SGMwiiSYiLwyMqzB1isEk2bAs72nZPC6VcBAg4vt%2BFyny4g2YstJ2V0NlXa&X-Amz-Signature=7884a9320316a3686e1d0633fa3d08a9fe0e6255060a8e001c7aa72761ce4e40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

