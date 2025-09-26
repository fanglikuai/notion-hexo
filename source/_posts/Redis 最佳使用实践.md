---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WK2RB5XT%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFxL%2Fon6Si5Uguus8vCc6hzW4Z7ie%2FWC8pcbeUSvT%2BroAiBV6aVPnqi5Gpw7mlXfgH8UGLZs2mbOg5gtwo%2FgZKp6KCqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZ5yIhs2L1CPAC4xYKtwDT6cMKpS%2Fxh%2FuzAv0uQIShMsmLDH3kRBMPOoNdlVjHZtKzWEQIiAwQulL5YZmlK629EjkuWR3%2BBNePoLtU5ucjaWtew2BwKXMV31hTpafj4NjQ0RiBkWlXJPONCzV%2F5tOhDaX7b4NRYxfH6K2hnfqVW%2BdsuCzrabsCkaPtgh8mWkeYbu77dQh2RQIo21Yuk%2BVUR7MQG%2FH0aIyBgohf50lSM%2FK1FhUfmPLgwOJ0RdrGLvzdC25LWb8IGspX1l2KCTwtcM7Xrq36phtrUVS0WjkxzDxs8mo0FymAUU6g%2FbVanU2CXxVYiu4JVClzuxZ6iK4GCMsNUSg6tcCR5u3u%2FuO1xaZXvegMY39t2QEpcQr8k6IJfWuIEtH%2F1wLV2mpbwVD%2BxUdIsKA2021y%2BOWKD31I1sjyp21TmAzByik%2BhsUYxfG1IHr7O9pZzzfikY09f80IONP17sM9GhUhgWlHdMEGTdxrlC9EeT2VylgtFij%2Bg5rJmArJZoeAshpAENOOROSomiTWJTcEHsHTFlfCFaGfABv7nhTUm6QySPIvfsF7DOk%2BikJLApM2Cr%2FXHq1YJEiPmIxv8n4AaEGt9rCesAEk8PkSoyo%2BCM1GuhMnpowFYMswcHS%2BIms%2Ftypd8IwjYDYxgY6pgFncrQP0f7rOw6RKMMwuGFA61iX%2B%2F5YaB1bICJ7FmFOmjBP3HohP%2FOAL%2FJ9VB5Jlamqg%2FvjwJGD5F23DZV%2F5WtfIKeGTYGgfiYX52AMcnhRx5TyX%2BNI8cSf03TH9IS4foiT1bYFy9HeO0eBCKuKRKk9EHdFZ5EbLdGZm0Iy887Rewv9xHt8bgESxgm7OiA7OkejC3sUy%2FyF5JgKxpOqfg1RxtpHRnbk&X-Amz-Signature=b2cc203bdb6961118c6e49faac455acfb529532412d299d569bf28bfdc36728d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

