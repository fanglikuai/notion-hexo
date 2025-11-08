---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XB5YAJZM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIDuAVVxXXBlWicaQCkSpkdn3KTrtLd77lC%2FtsX8gEYueAiAuwdYEKneBRnxOSzpfGErWxuTt60C7iMU34Exo9AYZFCqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOyfQrJYftgZCMC8pKtwDfwagpOHDH78MKDhkpN4vgN767d3iTMQ398yQR%2FJGzvnDtPwQedRTr6A5akwfyXIp%2FvRlpTO9G%2Fi36zAXOnDtnbPjw1uSMOOh9UB7xDOJTvq4zCtQs6cpf9%2B4DIANnBCbY6q%2BhZ%2Bf%2B8qd7d879RP4tAaKQfzBoth%2BOYdKwfBkSWLVIrigHWyZs6LCFOPhaGv3wuau1%2BAN8tGJ2bOIyPLO25XFrnDbczwHncJwRccZOTpxhdoaH2Qv4%2BgPtsvZ%2FW8YBvw%2BMLE2sB02egCeqnqebbHShpZItIQ89CJUU0Rho2WGY5LL2eGCWZZQep0xeuVBtm0j54ImwT0i3bnUOtuNaJ2PzazeZqvxemXk5s%2ByUaydQOQ9R5RU1Uo4E%2Bfp1uWUEKiQ1Dul3EaMaG2vnU4rnf6YiKPxTO3AmozSA1S2sxkG0Gvj43e6IKfMQ3kdxzd7Jnvr7fewi2n2ZWLIFHI4E1JSli1uQIm01vsdgdPvv7NyfQY479UPhkR8gCV9P8PI0nbSwX4pAx0o29d8jWsI4kPPY2gNQPO0r4tD4HJX0Ndm0KCbiQVJK3Z0RC8vGMrrAZIgnvQ2BmsHGxIDbxsWHxZXuGTVI7lSlMe9ZRVN800dquPcURwVfvj6weUw3K29yAY6pgECnjRFA2SuAN0%2FrNGAW8175PLgqiqRYYR6CY6I%2BBRIqR3LDU%2F%2FE%2F5Znr2%2FMnQJLjwtb%2BssC2cjkBoWXLpGKR5FcBUSiGWfB1BhzoQE0uWpigmcwDX2kqIqMG6CQdKS5IaeRPiGI1zeWSYf7YYB5oQSLZdz8PRiHlUZlkdq6gOq4cOnM124TzYv%2BlY%2FQco%2FNknrP%2F%2B4pH7n1%2BQ%2Fi3jthJeuLLTb%2FGEP&X-Amz-Signature=e277aeaa8d3b019d8047bdeadce576b6a786830b3e2f1f4d9013d7c164c51100&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

