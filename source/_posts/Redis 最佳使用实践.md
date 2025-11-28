---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVM2U6OD%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BitBZlb%2FVHZ23qMjzHhQwoYsWLd%2Fqe4kmWWMPMRqbfQIhALHMzTXHBKMFeetEZvHSf5dQwEOGkPGMsBXJ%2FlzBw%2FV5KogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy1V2LjVtBMZswOorUq3AMFfyYAKpOAoZwr9rrG95AaZ5M%2FTu2D760hTXI1qYPqLfU%2FP8jH4jlIFKPH4EKEcLQPKM5aCFTimlIjTaZ%2Bm2TbYBNMIvlYR%2BxaHR2pvZFp6i1bhXn62UezqOQoo3mcNDJNQQdGDADI6BrVnAOlOXj2hWTfHsmvhhfOobVdZZLblneGtzTCDQDtDWQ0Z%2BShTIMJSJp18BZ0n%2B7ILnCV1NY4a3%2FQVp3ODQVWRvr6%2FFcAimOKR8WOg%2Bb0887ALd6j9Q8OIxHEP5k8sxL45gqbUO6E0nuAOUZeb3b%2BIEgkRoBWcup96PY4QzjgB9oQzwp56LKf%2F%2FdfuMlmG2NbPfU9r%2BSl6S2uQHriODzHqiTf3n9QkZLkTbf9IfKClqRBmkacwwsIR24f2xUsGyvdbqyZK%2Bm6w7NmZJ7i9bxHhIHGYhAMKKR6R5%2BFX8Ao6h7qWsAAULcuYAc0XmWqI%2B%2BPt8fv5pp4sScPVkrKeazlmzfl8u75ahRf%2BjwunKLga%2BmjJfVjwHXYuUT6swlAq8%2FjrXsT2DxXhEbVcnLJLHVz3WY%2BqzFN4xKRzrVnrSwaPaNMOUa%2F8pCcACERl2zjWDtxU3HIKeY5O764iECQLazNXt11cgJ3KWniAuKX319Mgk2SSDCO2qXJBjqkAaTwxaqSw0iZWcSSwHD%2F1lW3tEoaIh8Gfo9aeuwoRaEcG3LG1fb11feIx9J2Z3gqB1GMUA2UHx54n18Z4vctt5dngS0VOLk4Bmr34Qf6vUw20eJ%2B2NjHZeekLWhh%2Bu7CzWtbtt51cTrWIRpuGfVUs1VTREFxELVGUWOafj08EJAlzWiwK40ueHdmYjZvjdHpeeYA1dTe1d6JZX6ebXCTq83VOcNT&X-Amz-Signature=5c7dad8fd607e8f1e2159ebe2ddfce1b140b18ba651dafbb416b1522a4023d95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

