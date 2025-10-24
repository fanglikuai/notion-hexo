---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646BDXTEV%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrFoB%2FmefvkiFK%2BKwlAM%2FHIfmhiitLSFj%2F7eHX97rrHQIhAK2%2BW%2FZuzeQfdREhtMmOEZrUWVJphX4ooS6Mqs3Z%2FDO4Kv8DCFQQABoMNjM3NDIzMTgzODA1Igx%2F2ju2lb8H7Lg0IZ0q3ANB9GuZRfe%2FskRIQuuWdeiVqxWFtrrLXMDfTMnD6odPIh5QscMB%2FruNEcqYuLARmVOIOckCPIY2Ftwr6aG2qCODNNo7%2FDjF%2B2kU8IlcdvlYSh2MkjQhw7Oxv9TUa2%2BWu8RRBw9MAlmMwVcN18Q5XIuU8Y%2FUCCzDeF4eOabkPykxg3wYyu2LYnSuc7QvZWsgIiT%2BSK%2BzlE2LcdYI28RSFQR8lnZFxhqxpq6hc2TScugiy8mE2J6vHLvAhANbDgcRCegb%2FkudT%2FZsTJSGipncxi3AreeoLSE%2Bqu72zdgxBjkl7%2FOp88iLAEzMW256DPICA27rh36pqCC7HOwCB5JxRzJCHyzFLgvnMBAYbxliZJac2ivtxCIlK0A3m92PMMYwc7c7QqjurB0HoPMvV9sz2k362voQfa1mQm76V2BuZLudpw66G%2FbIzB3l%2BNfEPCS%2FUQ%2Fb3IP9BtV5VlsAaDIVcjZCP%2BzvaLDj%2BA095jljva05O8Je8dnGoZWk2yiuep9jP3GRVFoccZOzbUoHrxF4Sl%2F4JgmALBP8FX%2B4lFZQlyRrZgtyPHkqgMoF905GxldmexZMzAK5PQprGvxBnd4YcEFEzIgrgjK2CBUO8oFbIncl0TFxuC7g7CGjrQMGBzDmy%2BvHBjqkATTxL4y6hD%2FK3FCz8BBqKiga1XfLeYZjGC39dJ5khdS%2FqJFsB1%2BJXRX%2BNbFbjABOHH1diQxRDvOOLc01F8cguy5GD8mdxUczz7DYWEVntYMtCdCBXeLZsgMCUTcruxM6ojI9sMyPNB%2BDujMKISuEu59HibI1Am02ueF51uApnj5%2FY%2FZxMGoGmI5j%2FuXiR8wF0%2B195AQ6AulmrSkl4%2FC28Ykus37a&X-Amz-Signature=ded6d22643b10d51596afdeaba85f8382c8260a54f566baab92db045d2591612&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

