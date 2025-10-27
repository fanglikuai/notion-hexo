---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWIQ76JJ%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH8yrVEfbhSalEfDp8k8UCCwRUZtP1L64ftyKAYPGxkdAiAzWqa%2BWAg0ylCit8FNc7xykTqJ3ayipOmGKv07f2pwriqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP1XY%2Fum5S9fVQx6gKtwDj6PtNu3IjIkCnXV7mWs5ImbZkMAXVQt63kYBNkI2B1bRNW5gvT6RYvPL%2FAJRJms6WH0ESBfwdUga96Z2Bw08Tw97vjBcW8M7AbGP%2BnvDuJBZ6W2IKNFnK%2B5nzoBaaX1e9V%2B9qzF%2Br5oLqliEVgLw3EPMA4kJ2cdoMp1UUTEF%2FiZBwGn%2BJdT2mGX426xP6YLHOeAcrb0qI%2FB2BgcnE1zikAkrdLCkyPHM21mbRblWpQ4f6hZZIwLrUi9md8BruYfNbGcvie3EokM%2F06jPIkQxRT0Vl1NVMkmn36mRqrxRxnBSdfkJeMnuyEXn3NyHNeFv1RS961zAPmcVQKy0VzQWgF%2F6KEMiFrDYdEVo7s3fPY7aciIK0YOcBg%2BU%2Bg9cF55DEnH0DqwRgv6UZxgc2qsf01XTKY7wrrbz6U8I%2FMD49J%2Fib1WYMtyCNYSsR2aRNihm5Kk5cHllRPAwuEVBhP9eiW3mVOC4DJdwJ9nMTk%2FBagnC3WXg%2FHWwGxdpZIYCnasJoe%2FUitCBSmkkOFDF9Kn1ijrblVRlX%2FH1MXi81lMu6W8sFiDot%2BkJ9VkmG1CX93SXarRCTPBZs6EhuiqSg%2BHzLvvF8I5EHNSwLROYeBeeCs1oUlqU0Sf9ikKDoFgwspL9xwY6pgG%2BPQlJQ3erVcnaXlHYF7vazYczn%2BjLkqZ6Qwh3iHNPUbQeHVHS03eBXnGx6km92vdWsBJk4juc7S09zSHCvyUmGakKdknmRht1Q4WQQjYp2AY311W9vS88RaJA%2BriMOEjR5LuwqpLnP66g0Nilue35ZQ8cSjm%2F7W7oC2ZC%2BW%2BrTZmS5nPpexpniRSdoByJ9LJjKd62wHTAzHpANKLgU7kddyNHitOC&X-Amz-Signature=68d890ae2744d20bd91db60dba39b8ee7765faa94cbaa3bc66892d756796e8c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

