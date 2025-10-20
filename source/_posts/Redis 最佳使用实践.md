---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FY7ESTW%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T090708Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQCGifxaSxWjXL%2FyU5AGT5I2wzCLFraG8EvbVBDCLl6cyQIgcCTxCrif9sW%2Bu6pGiixoEOA5ceF2XNvYnlX1vJzYE1EqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeoMNueR%2FnrfVKGBSrcA0I6buwmVPklhy47dTqWJS7nNmVC9z8Z46jpaR0Co8YxjHXEt4D7PX4rPyjpWcWmZByDYRn2ALUqUl0g4O6yHepjtISZ%2FVuppXXR7lCyIaj%2Bu0s3vjWdWijZSlJv%2FabQUdNj%2BDPTgIEUYGwgU%2FZvWanicuW6nmtXjQf8S9G3Cfyb0gldbZgnYKFQakOJrzRBqyfGeWCVfjttUuRriGWUuIU6wLadlAOaGNyhJmQUQQ6DD0LhY2eB9g3xpg86dJ9ItQAdbMT2lHriT9yPgLm7DlcTOVlRjYkDZwfNnhnyeczNT16FMZ6iewiRIejeXJqDLvaNquiuYKJtRUPHfFQC4gumYlBYedJzHA4XhAy%2Fqb%2B3hM0PHnGt9jREUQa2NZCGOnSyTSwptO4EoC52ylbK7%2FNAh6Um7YZ444StZXbZmRUYFgeeshd1hcE%2Fpmp1kksmiD%2FwgHwkb7Hg8ZoJkUpkJHhuflmLZpTYyIRwCQeiU191gO72ZP8c61GbxXPAnslxqxyG91iFgquLII9WauSbHzJiZws51v4qckqu%2B%2FF1otT02gS%2BDZgzY1tAS2lrJpd3VWURbGPWWOZ5BH7uywVTakwai4IiuqYtCQMh9BQVrnHyXC6RCNEykAFWmAnRMJK218cGOqUBP0VODZWzoPmdIN%2B%2FVdrVSSamfVOCHyQgDuB9x68EZB5lM8KW3Sj3WbbW0PRbaEDWiRpQzPQ9wS%2F6FzZop81k2tqMVUcWmILsCIIwlqq5AuTabCFVLbW9sQY5QNFoYoYCgZYd4NbIv4j1BoRWde5SzpmwhIZcF5VQYQzq0DTvjHBxggjqwpP8iaqE1KvoZ2e02e2g60wpiU7POQ7g8rIgj%2BUWLg%2B4&X-Amz-Signature=3942c377813dbbb1ca59dd7c8ed9af89eae344849b356edf079d2c7128439899&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

