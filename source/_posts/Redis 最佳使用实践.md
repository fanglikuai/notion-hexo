---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYQZT5LK%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQC8Qh1DzrCLf2V%2BZJI1LDCjFwdfTqGsjkZaRXZIJ6CU9wIhAI%2BQY4gTFJgzJ7MyrHOUhbH2voU4ft5eqRs8r6fCq7QPKogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz3lB8KEYhg0L7LqHsq3APORg%2BKN7Dt7GEC87BmQoISVmjMEFBhHz3KgkXiBYt8WRZNQNpJNGewmUsEPDkzB%2FQuQ6nPIpVE8wdLuGvmdg085J7ns4lmTCzO1zkA23HWWR2qxXEva2Kfyqpt5Iz6FUmlA0ScvHXB%2FnGeK%2BqUe4uKc1QKS4sPnFZ4mLeQ%2FCCbMPINHciu6P24mMobfyoY1tzagvc%2FZeMdohRn42b7jkc9VyimDorPj8GYDUoB4BV7pPyttfE0QTU%2Fd6nMY%2FEvAi2hVlHmrRsn0d49JVNfQh2tGk59SeiAxxxotFKLnF8hcNhsA7NV8QynI%2BIUnZfbZn2EBo9%2BmYfwnca5lMiB%2FKUj%2BWImnMR2Xpo8ddcmq56gaKKGLeG0bOfmDVNm3bhIOptMIiHeMfhfZ9x6m3N2jtCF20BvgyrKCy7s%2BhQb2pn5UA8jSQtgj0QCvCcDKXYR7AqrocI68jZOgcS5llhgwQ499pnahqtV9l71KvFF9s84a%2Bijf2SwuTwgV3FJBWsGGKOJAQS2gFPLzy9O9nDSAtVQ7nFRrxlpas30CI%2BsDoxQdknWw2QvcLmiAiVGvX4AwR75EdzO7yUAO%2FsET9dLPcBMu1U%2BwhAgTgc5Ge7eAiF%2FgP2pBV%2B714t6Cg5IjTDu0JbHBjqkASuNMr82j%2F9%2BjjabbLw7NE8e8KrckBRmrpL8tpTizhtrZFbomI2QZQdaDCIxodai6bHvJhINVCP05htVY1pvFSqT%2FNgndd102UlMpqxCGlbpeq1MHiNpd8WR%2FU4jv2kmJ07bCuYlt9rrbiuS%2Fvt7kbgvSNWbbGHXoeG4mEjxpPsIIU5AH9%2Fs0ft3T911r67DjHrtp82fNeDDJpuVRc6s5nnfbRKf&X-Amz-Signature=b1b55b525c3f7663ad013af684c301b8325f43ef78467bdff3a0047099af9a32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

