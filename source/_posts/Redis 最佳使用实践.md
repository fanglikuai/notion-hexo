---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SX6ANYEL%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7qGZUhRsuqPNsxr7%2BdhmaoqR5E8vSqq4rig67r1dHlAIgN5fPwCNn4HwAZGRKPt3mKqAPJjGD6AQZAbeo0V0Ccokq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDIP5Du7YfFO7BFGU0SrcA98Dx32FIvX1%2F7FScEjvhZipfY2fiINs35jJ%2F6YEf9YGVkK2KsTq2lFqAPbdbQw1W7kU0YopOeL6kfUbgCTceA1K%2F952AtNoPWnq3DqWv5Udm0nMGS0Xpg2EIGn%2FR2qIFZgrj9xLtdUoM9NQx%2FToLAVXOVhPPYtoDItyLvEZMdPMEynrlASWZkcCvZlZ%2Ff9HpxdQIGe5ZkuvR2WCfl9q%2FO4iwtqg2s4WEvM0axI9GqRNFc3fme6PW5JSyRARE3q9unW7ytbr4KNiFZJj%2BjmuitemDu7JOLGnrgJ3NcGjx2K36TXlP8%2BIXYx4Z4DnZEwKrz2MF2d6NxtkTIIjUl8c6S6JgSm%2FUV4k4mfA4DH1YKfeEUZyDtWvowOois%2BdXz6TE2Dvg0N95cNlmp5hsLQLKb%2BD2lYDwqdcsugZlCNmWQCiw4PYLMdXqHWVZna9kVfSGHhI%2FxDbOhkCHJFIAp6d%2FzTVcUPtSp6cDdINupGf5ifvNaNFgYvPUIApUXfK0zTWwA15hM0NlcMwhphg17sPibVEBshN0rEQ03MRSHjSBMZy0B4k15aVrPGAaMaEq8GjBHkGnsdroj3tmYZYNBp0pAHj6BbJXRgy%2FN888dXq%2B1KKaV1ugTCcAtBqUNynMMrztccGOqUB1Kb6M4Atzcxw9d2mCtdDUrT6msPsJBO6QEHpn0mXoOQI9EpbxQ5rgPWuiAZ4w83UCmuOAeJoJQW9Dgqxukh5vWxO8ViihbRIaVmloNicOR82kbdMsIE0K0iJGxF3%2BMV7JNJTt1SN%2BQlfEVwVf%2F%2BnsRCUSkxPmZ6icxVec5gKYGeQ%2Bk6f%2ByYr4uwbleXcd3cfC%2BD3dfWIe9D%2FdIbNRBdmOGS%2Bvojt&X-Amz-Signature=2de1b80ce7c4033d82b6ce323f6f0749970effe02bb015426e6aaa8b61fb8df8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

