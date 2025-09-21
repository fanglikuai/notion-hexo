---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3ETUC5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID5kekUBd97qhKo%2BoGj7l4JKrzBgmPuQbEgQjRnuQ8MGAiA90C2Z1D8i4MBQOb%2BuKOC6snN24BKV3OsMo7f3qtp9Zyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMaaijw2e19L2hb172KtwDUbGqGZDtWUx%2FqMdzGBrK2lqgwMSymlJlOQ0M9xZTQLrLds177MHUAGD7CrMNXRvrsrMbD3MATGWXMzWQNr8K1I9NOdzimIaqSLKkzX8t1XSoS4U%2BOso6%2BZEhZ7BuKZybVWfz8m8gRao9HdtC6ztBYWyL%2Ffugul3GK9TpIWaAKJAWdYVkoUrykYiF7%2BTuEs0Lu8kIz1X%2F85ZKwP5Jd3OJqVhIMovb6r965E9UupSrTgPPyp8EL9Doo8KafgWu1n2iDzn9ZFkP3fYWw3AtFKwJbDMd3QI0Wgn9lP5Sr0VZhbkC5i3RM0eCWh6WTfliwKbZ2SRTwBUkNHpCVhka6f5vdqiVVuR7gL%2ButNmM9JMtvyLJ19fpKI12cmeBX9A2xQh300BT101jGCXarGIggkUsI%2FMKh2otd%2BRYwKxCIljzae3Y1K5NZkHMr5AcULGdV%2FonwOtZCf4YMSVKf4QICue%2B4Elll%2FKJ8tHieXM1P2FopZ7rDUagLmyUSY7wfZbdSLz9zJyiMkN9%2FqoZz6Ef6hBkI0tAqDJ0JDGhvn1InnGwUeKETvLsfYRNIjFliOrJo1i1oYerBoYqQAKIXPa%2F9hU894YxxnZcQlWhDivQax%2FwAuSDKaFYVu3zXQb0cWQw3OnAxgY6pgFcJN2%2FOPK7zd6r9bSiDW%2F0nRiGWzhKPxq%2FePU1nbber%2FQ5372Wg9fEVdoWK%2BjxQa9VzQ%2Ffv5Z9bN%2BHcBlenMGjpGdrUKh1Shsszit4P0Lhh3Nb1o7AK%2BDAy3MMwknb71b6x2%2FtK7GEjnHZWKnozRD6sioOCtvRfJFCGgZBo99mvzBjnuOE1Ocr4FpNGScZ45TGyfWrT8imepYgRJu%2FrSSWGEZ9qmZn&X-Amz-Signature=82d50d6bbe70b7077efcfaf5717565c82e09eff395b13d3c4aa6e076a915179f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

