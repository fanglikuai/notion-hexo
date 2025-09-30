---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622JHCDKA%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T020517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIH61Y%2BNPj8zuXptWap9WVHLlvQCwqIZmP6Ab0tSFcbLGAiEAp%2BW1ge55KBQOxvDtJUZU9P%2BrS6mt515QsN7%2FAiw6NRcqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDPIcbxDhnus6ZYafCrcA60d7UjZ5WgRGElV1tobTkFD1B8LH38vSTDwnv6h6dsB5Cy%2FFC4umySoSJw4UJZGFQO9xlAw5s8fdToBabZUyyH6jSxyoqxadQBkwxKU4XZL%2BzAe2eZz03R3VuxibX%2FpRQaDJBRFRHjln7pNlfC5IkqCnb5Lv9FWAyDhTbZozXL6ASBOyAVuZVsqQFSr5DrDRqydk9KBe%2FuoIQFHpRSXjxdTNg1WM3BAPvynjdQpUNoc2xj0LHnvTLCk1Hdc0L3Mf54EqdFvqn%2BCyjgqWWeYPVhkuwiOrDNQSnTT%2Fwgiqmhgy5jicELlINVRGBI%2FymreXL1eHT0JS1mwHgfiOuNwviShozgKwAdEXwdmr1VLbY1HqwEUKGqu0v32j9FwTE54I%2FNaWmy7REV7cwfjUwy1tTooDWYmoHEwAWG26qoeR6b3I%2BZk107%2FYOgr6%2FcYq22zeOPI1Thab8cpq01GUDel9nQ7d2n17xlDC6qK91M4%2B4o2dSWD1%2F8dHlPllBz%2F5n374BH4p3%2B3SGcCAohnhaB8wW%2FC%2FDaHvTLCfsT%2BQg%2FCnWpK4LeGOVE6mCnE80Yuq%2B5gJtZB%2BpxKTDT1vXM49kWLtK931xWxDr7nuvitdYJ6sb41buZQSjRvua8ysrVNMOrn7MYGOqUBPV2LMu6qzl9LLvx6VVLtzwtY3lfayPm%2FJmJ%2FfrHJOv%2FvPKxJdu7JqHlp5XQlCse5IdJjq5NLDXwtU4sSosCKqB7l8OGMQGI9Ah9JjBUvny1YJGQb%2Fq6NbddXtYf69AmN4qJF3LHmZcU7PkgfV1%2Bbf5ZHQGAB%2BLdfpo7i8jXtdycfthuuxXtWDIVN%2Bal%2F8Pl2GewdG58n5mlZlsgDbZrRp4cp1hA2&X-Amz-Signature=2197d4bdf6b912b0050594678c3b9979c13aef740289aee6dbd33500bc968170&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

