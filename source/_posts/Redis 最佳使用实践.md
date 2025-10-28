---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMNYMYRL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCICbDrVjXZF%2FfRpY1qVffhF2xxvD4yjyG9u%2FGQsnNCKPGAiAUyMuUjRBDAOuobZN%2BDvhgVMNw7%2BY6q4b6haEfBqLPmCqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQdgqgy05j11xDHtaKtwD77NyLqb74mgo3c6Y9%2FP4w3iAUUo7gjWZ8I0ZiYoKWes6TLdcNKDsNbUArHYttfwfQcUe%2FJ6XZiLG2bx3LRX7HCUredWbZYUZXABGJtH%2FVxypqnud8PulFDXOLs8IIpLC1%2FLWYHQMV5DBuPjbVkRRdfdqbqTRZGA%2FgVUNscnJDrm1CfZStHnR7N8MjJ1Yu16EgK1hwm7xF9kOT%2Fq4UA1gkyzaxU25TOkY5F1Ih6sP39j1uzHh2nZp5lJgik7RAHgECB4JgItlOorpFR5zK6vNKsuPWGOZoMqDzeJoN9pBTUkLHG%2Bc7JHFKol1Wh1YgdJ5RsSwxKR144MBZf2S5nhzovcUVX5U7PIcSWhoFgqYbsywyeXqnD9sTDyOch4Vk7LkMcoWuHNynb97%2FgE8MhghEeGmEm%2F0YSLEsjZUz2XAnsrvBpkTf3fwRafAU5uPaIGP0Kl%2Bw5v9mjs04aP%2BOzOlIpQxaaQvJHceU5cRfW1yi11A9%2BPFtGlofSnp0oeQmbptCy%2Ft%2FKw6o8CN1SxAkWx7ksgp7d3uWmgelaY90pIXQztC455ya8B3ol1wIZknDZVZcXBb1gyS3M19RSUPqIFGBW21B%2FBhepdfWh1UMB7BvPDlGKck%2FY0MbYS7ynww48%2BDyAY6pgEALf3WhNdzvALJxt3Cd%2FAg26jVb5L651Lc%2FB8Xqo3t3bKqRF0E1DfLf53DyzR15SroMulwI%2FhmV6o2qosfJ4qh8fFmqgw44Zgk791hmo92N3mxlkvZX3mwZ0R2BqhcLXIZ7yY1Te6F6DpBae6Wp5r%2BrqtO8VX9RqNnQoSpTVKdfXL9djYMozTIo1EeGwPV1su%2F%2B6oizL%2F5XMary28T6hxyBvj919cW&X-Amz-Signature=c6bac11a9596e2a76b24f47be4e769c1fa1cbd836caf4ea7f7991243fd9d6063&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

