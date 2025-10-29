---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SJL4URA%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCmSwusZmUbuFTmMwF1qwchWCNTe5Wtg0ZSUH0pKNMuTAIgYRY7QJBL26G%2FGyfmR0Vhvx7DIS4tOQqEpVAZCwYx4XgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOxBqZYd%2BzeeM2iJcircA51b0vYWWhaYkD7PoAP%2BaeRa2J2h4JeTLl%2BKhTHVvS2afXlLdJcTNi01sdYxvnGPfMR03kN9D24t4IIVv2Le%2F4zROpcmxDlvKhBhnuu3ze5hHseEZZFp2F8SnnjnMDdIcMZqyghSWuDY%2FAXJX11BZOQxwJ7Q1AS2nKdcs%2BTO%2F9VekSWefseftUWe7NCJ2JeT7HiUxx96qxVqux2UJQVt3fciKwkOEWaaFYhaO7gz6GhY8Jqq2dQAsd%2FYfgTJTin8DiZc%2BIMFEBJ40XfvJVIkC%2BHyV%2BVBNW1DkoPlFXfEMSeJFtaU4cShVcqMgwCMFxWrGbd5RYM7J1Tod5t5N0Eh36QXG5hweNJi2san2YA7MBC7%2Fs7E5rKVm39Cibly1ca3tsLZXLTH%2FNpeNK5UEqLT%2BkvuUed%2BPhEmTsNGr%2F1iJt2AQEtuHJmMAs5E0qHsLRmbJxnDQStTVJV2UQcVLxsrquDxh%2FAH8VtM1%2BBBGqs86hR76KncTohL7s3SwrEMs0CDkOKQl1%2BudEiYh%2FhwbhAYUgZ6eo1Q17nd3JJDClitoRwwerd7SK%2FSCIOSToPb2o2nbYf0fXhW9g7yF9%2BeAxudZ7GUnGZOLKELZnQ4LAiqLFBWnByBSO%2BRFfJLd1MNMNaThcgGOqUBQW%2BvqPtv6A93LFSKH5DS6m0SjBtRwx62iico02Qp4z9GugHu1%2BjPoU4vxJN02td0xx7FA6ba5njP4NEvdKmijbtG0MZI1s7aOZ30vZo0kzklNdMBW4ors4gnRLeUbfx0e%2BCQAybDWEJ0azeNadn%2BChgZDgq9gW8e0oxvYyd1xvLPMwaVAbuEuZyipV50OzOlsgFp8KuSQU74o96g5patjTccbb2c&X-Amz-Signature=9364f05e62aefffcfc5c01f3006ce931035e99395e971c1067528d7d941938ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

