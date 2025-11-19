---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KV3HOKX%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIH3QdDxT6Y8u782F0Z4%2BtEo3R8ECwkQ9drSwx5mp9tvLAiA5Hz6hc%2BUJzg%2BmmuIFlIvUrxLKgEz1nIcLnFBfCFpM5CqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP7oj77JTNHFqALu7KtwDl9qIRux2BGR2TsTKVS2prR8ZjHJEDszhS05V4q0MYDPznRiD5DBPsEWkyoxJ7feQt5niMqHuBya%2FESplQsUEVQP6w6DdnmB2F7cNCQzQwKHrdlBdFyFJyfqhnUfVTriGTsA9vZ4s9WQCawgbHSA7LzpkluXmC3x%2BsOG9lnCmtkTx%2FXNmkBapCsrjwF7dvN0BuXzWTdiiUQ6AmlKxc3571JV4iyXR7S7F3bvfs4K%2Fe81zdDfWdXHjYB15Ny7Sk851CyyYuXpVqKZgnrI%2FVvJ2MS1RYjnLL9T1ahCoxaZPv2oX1vphdboowFo0B55T2nWZKPvfFPx7t6L5j1MVA3jO1abJWPY9%2FFkEKh4KGDUbFFtxk63g2atLHQjItnkm1WXa7uDJ3MBeVCTT5Fg7rNXs4L%2FcnWMzrvUg6Ho8mXQ1eyY9qyGEP3c2d70NV4WXJaQx95Di%2BCqpIEvmZu6St4trQ%2Fq7r2fiykq9CMmtZrY20rWYwo3sIt6cjZpiYb3SJD8PUqv5s4zqyORPaE%2BsPSyvV0oHcTicGg42YC4ChPuf9VLF9QBIkpwISAFpJS6%2BChSdPDZAt2MT9y%2FNcTd%2F%2BWIhHqGC8DhzoerxK%2FO7bp1iEpBi8Eh88T6wIPrvoCUw79T0yAY6pgHmhKzap%2FNpBMJAvjeWP%2Besldti%2B7cvvyosZKP%2BJDScdHwIsVDXXJOCdDoBefKo9n2WvCqeTUCBfzi4yVYDK7MaFx0HWPCLLdM06MrXFJljXbepWFMXdrlmuLj5x0MNgw9jyRn29H3kQIyx1D3yFxC%2FHoHVFDMEz7ptGmowLPnwKRW3JtRVS7A%2F9AKkt11bZlDsmgKyoYFdYPzYYzOmwvWPck9OlO7%2F&X-Amz-Signature=fe8f92b276295082af1c8d0717822ee825f0d9d397e9e3298565bf55ebca4e42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

