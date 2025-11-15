---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE66G5RS%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCAhm8RLrW9pldjIvBGuWfUZz3ojZU8tGAA09o7bwQ0QQIhAJ%2FQD4D2%2B0AlcoG%2Bc6kAtUOkeydri593g4vFrpSlFVN7Kv8DCHoQABoMNjM3NDIzMTgzODA1IgwNTFXtKvEqssW6a%2FAq3AP1ns3q%2FIYtSvz0KmzwBgu%2BV7gwwOJg4NHU%2FypbgaBHNc5oCBU2g3rO%2FHqOc%2B1m%2B94n6kedt9TptqJ470pNNbtocdqhMMuBQQlb1Cs9IEOMHqRisMShRULM62bsK8Lk2Zbc1kimQBORMuOfjRS3FHqYu0Rfzcm86bancI03YqBBh0v2j%2Fpc%2FgNCmJjaqSVQcA5%2BsDHadk2Pc5b6WH8b2XihkqNEJRySXa2iYo05MAp1m9y1gYFyfcjy6vmyT7x%2FpPm3RSloAKqTzus4An1Z7xmVzLazLQUFVWMOImDrBI2CZpYSmc70hgA4uTlXsJkwawF67LIMgQ5DRfYx0RQmhYLS6f1AA8fVCnmGRf5VovtuP54njNoUle2mn%2BQaBTf%2F40ktL%2Fk4QsLH%2FPqSmns9f%2BG78zTGDPH%2FDYcbuTvAkCIurOrX0tBSq4SYt15pCMPoUR7I8tzq0Y24dt6KGiFMYwIuCX7MuzOJR1ofQJv2xzb28xx0EMNfIWjd3iCuAqeXE7k9c9juAYXza5D8Vy%2F2%2BFJL3Y%2FSJizys9x63BPwnO0tWer%2BrYL6E1S3%2FTDAfUOUtxF0YVXLsC9yNlgClnMqAORf1HJbZrezloYGzitVocwPJS3MeZE6vJdVDaNc1zChg%2BHIBjqkAaT1LKNsIbi%2FKC%2F8%2F5058h%2BqigRi19DWq32V5yoOdpAfO8CdM7VfhmFUrxG%2Bn4PCZtETOt%2Fx%2BYDF4o20drclVHyf4VUIRo7j2Sui039ot5zQcbjUbG9t7l1Iu28RZ0Cwlj62o%2BKe5NAZC%2BTHLKb8wOSf9lN9%2F8zvo38YuV%2BUVA0xSNdmOZZsCeZhabVmcpSaAE5517Qxo9ZHMzfa%2BgAFxVDfwgyt&X-Amz-Signature=ead28f0237a818b0e9b93371858a5416d35a9499d405b7539c0795f6cf6c6271&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

