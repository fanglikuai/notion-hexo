---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YKS7H2J%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQD8%2B5DaO6qs8gRrPhu9607vYfbOT3TeaLKQRbsqFER5FAIgTvTb2eOnoPCs4ZBdowZ0uI5aXYZRGqG4%2ByG40m4iSTIqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBs9RdtWB%2BDIyICc2CrcA49H3pFeLCGJrDeNC0cJgXetAm4GklFGK8chYu09Q%2FmFh1nHHWAkbHF4H1ezMQjPtdU8obMpWlEzfIcikhdMv8bQ3rGukBYqs%2Ffs74rznv%2FQvarBjTuGoUq%2FiBfLl55w1YlrcHrHMwA8zq%2BzNmkoqyKdTKdM%2BGGSzsBXWmo85zCzH2fD0Tqv%2FIUHIdoaqvnCu497LHeNKVZ1c%2BGNrgF%2BM2XXNDORXWyDTVdLPkH0Mxa2aK9RppZ5Zq%2F6PIcLmbORJ8PyHpif5Lj9%2B1Qw7%2FbEwNSaes5E72znE883nXW%2BiUGEoPiSR%2BXzsgSHKAO6y2yddJYyGWS5fs5vmB8INTBNgWmktFHt1DyUWGHRLayGoYkZ92X4h2l6%2BRFWHKqMgU2DhdV4evs6UK9zfX%2Bky06kIoXuqe%2BcpRESb5M8AU9avVw%2FEvhR4KCoKkoJQv9GF8tkccJ2ElK9aY5xs6UacL8Yh%2FeJjjgrQ9Z2P1Br04WdgcRtsjwV%2B5X6rSrFXy7fbYdSiZ4K%2BiUBuDLp11uZxZnwr0jjSvPQ%2F3HvCxdJslPKlsAB0kAR2uTcpamkfckjcDU2HVUTLvbkYbsKgiYwzTULSsynJTM%2FqqC1gihWOwx%2Fyj8CVvTmMOLzTG2nvH6GMLfR3MYGOqUB7gcPu3GF%2BmwYSSezVlQ0KxEBBjcbrFfDBId0%2By%2FVvd7csM6feE5FLcbIchdbzIArl5M4idlpJSK8Qg19Ah0vjZpJyi0UZRFE0RAWGUR8VvYvBCb3TqRfXQe7wEzWfQ3pJWxp5SqLj6VRHQCL9TuS41gd86DmT6oi%2F%2FOV4RZoCfZdBT77SsRLbkuAi9jxrSp%2BKxX%2BtVSMV5g8Wa5s%2BlTCU0d3bViH&X-Amz-Signature=a61944a22ae0a11a8088533080fd9053c851d358428089d880e5c09b4d058722&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

