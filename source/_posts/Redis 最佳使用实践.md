---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZYYGQF7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T060139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQDeeZMPUvh7EDWZlctb9rVrhzQo2yVVa2r5o1a6Zrr97wIhAKNYLnOcjeCGs%2F9HpPZm7QBGYC3N9p%2B6KHt6ekTkFXTgKv8DCCMQABoMNjM3NDIzMTgzODA1IgyRzb5L%2BIbC8%2BTtuPwq3APmIYXXo44R%2F%2BPBe4YDzUJFJRxMpC1K76ugLDY4B83Y0btvN2IyJLDbmBNN3hbVvmHWbYzifPGcLRW5Khe1BNtkguOIQO2feN4bbvnLjeXbLZVStdMBngAo5XshjuPiHgWKCZIJky7IUCYLCKvcNjYmxplAjCYmQ8N%2BNMMt0rg0hCeLLfKArtzSWboTRk86UaoPSFV%2BP8WTGivlE3qe6evGJbGINJkBsc8P3LICv6ZX6STE5FmWrMOBZW3cM8hTMVo2OqPL7SHP30GlZbgT6wS27pK2bP5M%2BGMg%2FvFkaPfU4Qz8s%2BTkqdqsBcZX9qWojb4tCY8Z4neFnOCxDhw4upovJpRoUtVvD6e%2FQh9BQWG5tZanwSaQOY5N05RfCY5BemWTQ2YOkhmYBBc0n8TFnKtE0FODGZHGQRwy3WH7u8selSD6AWKhQqvFnIP4NlFaqfmm1UBRF4usHNRDyfiwCEHKyMdLgdW5afs3H8tfzVemLgcK6ylO6cUBMhiA8XZN2P3nEXXM8ZyJJ%2BDt2lGF9WZHi8WwsWiSmiR7IeT4S4eZsxeofL5aQN1eIO3o2V%2Byzks%2Fe2IKsGbAopsXnL6LwrXR%2BKiX4mYxNqldmw8G3kM4T%2FR8kYIPPzOpFdWHqzDQ5%2BDHBjqkAX%2FLCb%2BSp%2BAb%2Bpj6z5osPMuy4oyuoDW9jhENbnCsPeIasw8Od2h17l7v5H9%2FeCCWwdCdE02FRlMQaZ0%2BDm0Gh078RKgB4xnPdi3M%2BDJxWGnzleluX04csYYkfyo%2BYlARHACsjreShKBrybJbG6zGVRBtQJGNa4rDA0oqvsBelzllxAadYpEwUWBkIfwfOybuRd0dT1OYPfLqX7BVG%2Fu8eRPymzc%2F&X-Amz-Signature=665c98e68e63b11f96f10ace11d905809fe4602f41b322362bb6e5eea75542f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

