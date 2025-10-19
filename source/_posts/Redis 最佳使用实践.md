---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFHGNZUI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJIMEYCIQCgkKUHNjFxZesaljgk2KH0lIThdbQVteyvuZRrA3u69wIhAIxm3SV5I9UK7h20aTM7RYSqCKC%2BbXfQ76RH4MnysH74KogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz1ybjLXMLnOyarysIq3APNyKh9ET7jYD2lmFy6HG4PqlVZVn59hdj6C9fAWgQ%2FXZMvHWW2De7tIRAEKtdme%2Bb%2FIojeOg6xhD5jbqbHCNLOC3MCX9RseLW%2FTgbV8Rgu9Rw%2B5EyQw28eeb%2FlY%2B8zyvg%2FquJl9PsI9kFVJs9GdOvcprWUjrFhRRlWd%2FIC6wGHuvqietJFFx6DFoztRJQ5BCBP7Og434E1pTBI8XHvZHuBAgQE5UOza5YmGT1abrfzxFVs%2FptxmfQ8g%2FqkhlnPLrIWQ9YrQuZfVkqIHT%2FZhXyh6n32h0yLqDvHlqPrvPusASTSk60BRY1a%2FcPcY8V6IHVfubPf75PXWzeOmlVoTBra4vZm5BIHY170%2Bg5D2QL7%2BPoa2SEGe%2FCuaM8jsSlEx4zj9s8nmxqGR5j7TMsJxmT510tBLmMoEfpuaHXl3chn93syq%2FllfbiIOySuMVLzo5jSM%2Bugjwd%2FyuM2Y2jYbG3G0Fjzhmg70c1hzx3rz99J0njKyvRW2I1aLDARzcOxlKx%2BcdQgJQ997AYkR1ANxZaYFRZkBNK6%2FGgwV%2F%2ForGBg%2FL3LHkiCZtfqdntn%2BjCl%2FMwTXQiNEwZjZgAEM412RGR3XbIdQenEyJWb2P1%2B2bjqodPg2KaWDU5XpgqjJjDQ7tDHBjqkAQaBacW2AehgCA1pWmVM5qkDVWYvr5u0ydvc7rEpjBoYPshM1FPKHasGv6eGdxpiMeiJ4kfoSgI0FVoqC6sN%2BOR0W7VMqoMy6gId7DSHG3E6cplBr%2BjCNPBxmAzOysbLTptPkKY897aZMF6lvv2iIM54vnu06iGGnDit9933Q1XV6agFLN0LEpXrBcyUxwLU%2B1ovbk6vUyXI0jbosJEtV1yT7gMm&X-Amz-Signature=86bd052b054d78f8c7bf1b71c8bcb0e0e739ff66a16b44302ed8e912ae12202e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

