---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2X2K5HX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJHMEUCIGFurbuZL%2BpD%2Bd2FrpoIxnB4jE1wRcr6m6nA3PUV2xR4AiEAp5aGftoSTuNZEAxI4N4Y2wYz04BFkdj0VIpK6KmAmsgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8MQeGO9LFjPS1myCrcA92Zaq22WxuEpmDSSQsDsB8Jfsl%2BFsvLOiBAWWoFaeTHfgyVCR%2FutHubDL6LmElEVAhBg97qcPRI7v%2FR2AobNoOVqbr2G2lYdzdUFkBPgyEtULLo196pecU5aNlBUzmU6AS%2BuGixz5PElgdYbqMytsR2sGCnY6FZb6ag3HEuG3%2B2z1422vEWv3MwXHZmbnAY6O%2Frqag%2BCLdd%2F5mat2vBEyIweQvs77chJ9iKw3P4joG8xMHs2oPRceKleMbTRIMV%2BJN1bmE09mzzoCQXpPFZZ%2FjR4jmDZLOVgP5hZWG4ARaPkKYwEZ8yrAsD4gSjx4hS%2BtV397XAx4YosUom7fL2PcitNkBLOGd3BPS5U6QLF1KmNB4fzPjVmvdrH6WnRrc1rOi0ACn2S84XC9rn6znPra%2F6Uhqj8KUKXv8B0QDdniacs6vcBgVEfpxZ4Gz3Pm20ZXUI%2FjGeEC0Mb42bEfF3fvxmCWqcCSLZXZClozmajF%2Bl5KnKyOZc%2FEJdgbFxR2ysvSfxI1mo4VtrbO9hRMCrrncN97yg89590UKAzXB5jf%2FSnyPELEGwWi4ccUa18mC7lMcGjBCF7gVzMKlHErPLHipeKGofYLiePNudCuV6RMwyh24r5rCc47mbpjaxMPj6pMcGOqUBwFt73uiYCh%2BrI5LeZxuOnTCoaqther5JdKijImB8QRWbi%2FoM9KsGGeFt7GkjIWXrseR1ZQbYRJyH1259ShrYmYLOhFaWoNaQ90KJeS9S%2Bm21kPM0TJQxl%2FYOkb6137lLEDYV0IivGZjNqokopmv1Ntq0bnZGS42OUfxVwNLxRU0IBa%2FRvyOSTI27DCrHJCfLPxUdU0l305GC3HrJPdKFpvho%2BKrk&X-Amz-Signature=b014197b4749768696e4ba8b7f7b5a8399e645fa1067cd1c7d80cec510496e88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

