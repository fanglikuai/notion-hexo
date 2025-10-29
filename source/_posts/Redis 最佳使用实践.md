---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQBENZJ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIB0Gzh%2BGTibuCtoMdoitemnITY3CDIM7bVPufStkHaSNAiB4dLD4iAbkY4EjHmxR46r%2FPijl8uLey2ctni2Xziaf2SqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMz%2FrmW2VQFDYPau8FKtwDscgLVM1OVM%2BDsb2WvVMugqZekvaQcpr0Hu14IkTxt2LJZxq3P3cMZUQuTx3x50ZFQY2zKTcGY4ChfBhDPsKvmAO5oYQ1n17h2pb1uST9qMq97%2BrMu3dHztrV4BD3qQADP8oy4idNazeiB4uNROVXxpgnpYeaOzHML11kwbqdyv7JmzZbOErwpBAumeFC8INZ5Lybs6X%2ByqP%2F9MfqQFPCj9E8ecA0gbwpLgDIXKnz7IZNYFghjSzzSfnZt%2BZ0rDtcEUVTQG5gTJsMGVMKTIdOQ9HF%2BIbYhGiAntmcqXwuG4XgJGNJ7dbQ%2FcjjgQA5Cq2NYmOPHtMPYmK0ZMb0WTaJMifih39B8EfeukQYBrorJrDboPjlbF85C9NCOyppOwfEpmxTrYjUGOdzKg%2B4GQBVpdyOG6rvlZp8c3qkL6gIxNdHFiXl0jcGeZC1Kk9A8G8DsuTICsPGbSYMgiulSfn6SJyDm9%2BXo7xWZRWP98mh0EcWEoya%2FYClVVa4hqsoAgmhHuHU4WA0%2FnOcsBgSrm1YIfmAUpTbOsEADarwxKDy64GHoQ9sQXds43plEd2bFOz%2BNG7c%2FjDojM3plJMgFJhjgdbGkBTPwQoBAK%2FGBhFfyhxm0fwONto1XtgnC%2FQwqpOFyAY6pgF62102hdVde9vlo%2BFsCbp8i95xukRQjbB%2FHQuOToJOFz3omyiAtJ45dMwlWeYuzIOKvJMs2PI8q8U4ooTPLUz%2F49%2FmYnWBnwYNxgPPn91hQ3FK1%2Frc0ON4hvQd%2Fw4DPJV2CJu3AerDl6k9LOUNoaFHHVzbg0vYWggTpEJeHvtRHnvuOdd13OFWwSXrNZ0E4y3LYOuu5kyU%2FP%2FDDEOnjLTIEAmmpYCC&X-Amz-Signature=c3825bd76801179369cc40fe160a14392feff4dea77517edb188cf2c3bab3a79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

