---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7HWVKXN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T160037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCnRTSqvHg7aJqxFTGD%2BlFhJAlPsGKuIhlU6W%2BwYZziQgIhAOndnz3kEcyugRk%2ByrCh27jFmNVQCV05lhjXNxpru%2BubKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzJ4BC4GqPEVgDpzH4q3APM41ajo8IyQpwi5m1IhnbAbfBgG6GfTq%2F7l15g8sJz6HpOFzimF9F5ABr75vFztF1DDzjQ1Fxd%2Fhkh11nsPOeDnljo6Y3%2F%2BsgJu4HZgwoRiYXgCxb9XP4%2ByCosOp3slyLTaAoH8lpWMJ4jsHTLmS%2F8PomJ0fQaJZTJ9aUiYKWK63HXRlhEv4G5TCdRhNLjB4%2BDJihCIBmrqIrlIgae5SB%2FzdJupUyD%2BgpiBzkdt696vf3SyXTJTYsraKIMrO94UdplI50fJzBRP5EagALBpKpnxxrFt%2B%2BabduJBUIesffJdqBNuPTET%2FZZ0yfeNbz6lGa3VeRTnafKowg5tLh2ifE%2BLSnOFTO1x4Pb6o1Ltx6Ur3u1lE14IpEua9H4vsfLT1tBCnYAHiHfoytb0%2BwSTDMHEw9ZUXGrG8Rr24h0SixpXNMSF5txirlqgAQ80ZwABcrx9KaR6nCKqCMUORh89xjoGJkpYup6Il8dTxyO4zUBNLWNUJao99rKkCctP1hk4difAA9j2D1tVHSvznfpLoaI2IyJkqnKZHQ4D2kgigjG5UsFCbT2uJGF28r6MBNmykkqN13iwt2XzIjEFxnB0ObEEVdpdQJ%2FGQ1emtk%2F6jarTq9lGpLaEj2Enj%2FWNTCXndPHBjqkAe06u6L9MlQD7fXVkMI3DBl3rr3dWQCpq64m4z2Q0ber7MWrISQDwcXeMBJTa6URcYGlKZBDMuTWPrP5I2BtQrLK5lVTWNMLwUOPu%2BEwxAdl5s4G049sU5LckyJd09fa1d23ROeOAVwbRybx8bTrW%2FLnddeJpDg2ER3TsxZQcqJkV57avSz5Ipjj%2FTLYBlsayi%2FzmgrDpHgeslk8BbB2SMlmJJRp&X-Amz-Signature=d57400e6136dcd2188717ddab8476c54e51923529d0c978211a99e4b03c50a94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

