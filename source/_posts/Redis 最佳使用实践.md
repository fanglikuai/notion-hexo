---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBW5JEZQ%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSPl%2F9wljBKr8erFNFdcRxe62HDWozqntuqcJbicilCwIhAMqj7tkfYw52kF7lWHG2Ajzbois0Ipz%2F1iFRtYXuLQcpKv8DCFEQABoMNjM3NDIzMTgzODA1IgwuBqtu0PQGOxNNJLMq3AOWLzZsByrJ%2BnHlix8wgaGfZV4tAEQxdHiiqNuGnbXz1PDkdn7wpBEMT7ZWj2uKdokMWCpXuvHXbRczd9G7G%2B9Vjrp9HoDsItJDmWl0G6zBmMuiWSw%2FhDBT8zhZFLIR1QBivGy0IGwJGy9%2FhQYdaj7qd8PmdXmbi4ncvk5PSXnWqxaA13S%2FbnRcgy%2BjCIs9TPQiPU%2F74r4ZFTegJ0IxheJLOlrzMGV1lFpXv2ZtvnM4J4t%2BPD3kjFELBe8uXBkBo5q8Lp3fo1Ead2Yvn87D7t%2BCCTO0ywMDK4UQer1JnUm%2Fo%2BLGcQd607mRE7gcEFvsYm%2Bnu1TekIfulUxjTtpO9C1v6KOIbZtNicIdFP%2BmzhGcQeWUas9xtI5y5WkaGQyD5yRNqzVmPqBz8X4B%2FH%2Fdr0mwPZuSufbv60SDWh5j8aWbTcxYVaTw3pcoX4TvnXjarFmUu%2BpAeeewjIr1p%2B6lNbPs1oUxK1TSYr6t%2F9oww5pAVGWDhkz9wAMykinNIRGEVzIhMG3QERzWAA%2BLiDmFrBn5SBqPRqLZEFOVBgcN8D2TgX57X%2BoADIsAj9iPs3kxbmb%2B8Sv0KiQzIL6vWoy1RkzbkBE40T4uhBbvG6Anys4Y1mB%2FBBSPOZ7xY97aMTCSiuvHBjqkARSLe3LZcldd1%2B4fAtp07g%2BSuCmFQvJpBcQE%2BXOIXZqzjVc9xUMe2kmwoaD1KnCOoySyRGHPfgjftJyNEBfvJXbs1RDxmxpDshFAdvOh49QTZoOAcMDcPsj27ZZQurhTK%2FqC7v4QT%2FsLJ18R0ScTEElP%2BYze019lWnikYHdE%2Bv%2F%2B4%2F1VXeTY84PAwH0xpH8a0%2Bt7H5OKT40gUw%2Fr7Sggu6EqGbJ8&X-Amz-Signature=4ff3f6b2357b310146a3d371125745ace4e24288c420a1050ce7e14562aafd44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

