---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7MMYW76%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T070044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQDL6nc70aiP5TSbA9FeuFSn8PijkRX0VpA56DdwkKScnAIgNMO0KonGjMZZ8CZ8A937MCCoj2xDumEp9iWxH8pPC%2Boq%2FwMIEBAAGgw2Mzc0MjMxODM4MDUiDIoRyU90gtDY8sYwCircA1%2BaokROhwW72RT7%2BgzetV4whTJsFCmLc3mgt9iodrHbRf4jsUkf7QjPKTJBk0AXKIW5nZnR2hm228DZgvpRIcQlFQ6RmWwaY7bX%2F9fp%2Frf2N5Bp%2B%2BYWtBDNWawJ0RX31WzrcZxhAdjz9yBgq%2FNqDiOZi0Eh6f%2BaErdjMPUWziYTvfBDRO6HGOebHmtRstxOVZFMt4x2IQZ2heD5MQxTuS7fqL0TSSb%2FsJjBfnl5hkToyg4d3UUy%2F6urfUQ9W8aMF0wE7PxuZ9KMsmW3aLlciWsiPquS7%2FmrQzy%2B3WDjFBkukz%2FbAdVTC72JAUgIS0nwcJI8jj%2FmUTsFBek6GxsOQ%2BOziX0gMySZdR%2BNyaQ9cj%2FVc%2F8jbYVzH6SDwYE9pR0DbhFskgX%2FtDWv7Jzu0u8D2dOX74Tsu2ghHGoyTVKmmmfEMoG3A6YisYLRCyxBwGXhzHT7ZRlkcUnPXmFB5lLPmKahNwZfoM79Ye%2F3uZ5tRnBAheRsMw8hbhmIq4QAW09Q%2BgdmleddLjRplS3LaiakfQp8Y3xi2KJ%2FWAOcB%2B4rf7vMpmZgZAAFE2hgi6yfkUpOafkXGfnd1xDZN%2B85rDagKlmWoEMCKLmKV1fiF0Ok4me6dYNidBxTbiVkogTbMLWrkcgGOqUBm2K6Pjg0GAC8EvR4FZXEmw3imzb6Eg5lB2tREZzs%2FGqbKgiqI7aUkwS%2F6jKydP8R8qWSQgrqbxdqBcGWhH62o0aBqUM5a6aNVzFRfnzYcbCsK%2BjgV8clbrvYrXzCePZZ7a3NPbDHnIBKGvknGwg7XoXJT3CkqmE%2BDE4nFuISyvpLEE%2FrMK1xq1PvdTnOqFW5jyUAlPlAyCJpqRITrlU9NfQv64KL&X-Amz-Signature=49897ea706d255c8699e80dab029822e5cfbd279e4118006d878684fa03ebc62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

