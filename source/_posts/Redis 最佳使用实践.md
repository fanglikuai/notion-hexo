---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2GC6QSQ%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCg0ohHdPaOzH%2BjyPG3fpKsNygxKnirRMDxNv%2BZ8tUNRAIhAN4UfnRzzFyQ0afMqedxmDm%2Fx%2BFHSxctaaVwosjGX7aMKv8DCHcQABoMNjM3NDIzMTgzODA1IgwbE0ONARHujvGqQ5cq3ANf0bQV2oKCG3NiUHEjZaU50tIhVbjAHMJquS0%2F%2FrhgHvfwFpYHpQ4adlkM2hYUUck1TK1Sb9N1ggF63cKMVHpESxLf6dAZpgSsM02K7crpxaHUaMh9nDlvk6PQbm3VbK7T%2BoyGnFBf57xP5b7A1EZcR9QA0vCCGCtJPZQ7V5MWu%2BNPbnWu%2FQrJ621NxppnKMsee2dvz4%2BKZ8C8wZyyH8mH7Lea8PTX6PMcmv5vTWqJRmViJo3H9agb1o%2BEyt5CW6nCArHwAFbeerBTuQwkJsHvR76xK4GE%2BPAO%2FGMR9MoNInRHJ%2BfkCV9tCMsTdev4KaZajOmhhYkipwSjHckQYllTxJcXEzXpOYiNgxyHxSLtXKt2qf1pdbOE41dvD0qN2iQ9iJwVu61GxNWVpmE2Re6QtzN77rF1cmBxm2gOpXUzk0qQKCv4exkYqjKGcCaXCv3%2B9bGj1DQ1CgZP91UNsmtQ%2BaCXhyaEkjF0ab7aQ3SELsajhn8DD23%2Fvo8PDUXz0HSkqx49JPYbhFOVtYXSL3hyHJ8Z09oLFKYC86ckIzKB6M6uNAqaNjjSKZwas48cFReJneM7cR3%2BSwXPRVfq9A%2BgHG%2B7DYcDU8GDycmULUF04JTtyZIThQZEgepd6TD4itXGBjqkAdSCVBGvwgGkNIL%2BezcuyMPb35%2Bs91zhCRrL63eCmqDA5W8VOi1n2OErJsXNxiil4Ab08DBVANEEu42G4PPqqFt4SwhKeBXrupqX3utEHloQqTbCQGGcCm3NPFKn%2BfbXr1zBUQLfnk6yS0ZWbXNU76aRSAEwJ1ApYmgXpKWTEgzppKav%2B2nvE6x8fW62%2FntmZ7KpltQ1DkYUM6xHXKjPMW9pvAHG&X-Amz-Signature=0062e5456c831bfeddc6e091ee42261afaceb22671fb61e29be31a7922626b1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

