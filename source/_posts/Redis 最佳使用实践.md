---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q62CNWDT%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHHH7yxhjL9kOZOS%2BOjzDUN2KSOSjggzUEaCi6OC%2BuKdAiAOH68d1l7UwBjB1Kdd2wxvN4zS8rCg4M%2BBuvSVYMGODyr%2FAwhXEAAaDDYzNzQyMzE4MzgwNSIMg0CWq%2FVC%2BzHQ8vrIKtwDKrK%2Fe7eaPxQQWYVslNebmK3klJkAyASv%2BPDCNxuDRcfQGcbW4jYzL%2BxiD%2FnkK0Abouu%2FHfkDhV%2FJV%2Ffc7d%2FfBqrKhcctSa5AOgABVx1qnLh3hyuRAa1Ls63FczGpNYNMZMQuhzJc6tP%2BjOrzvny3usZSPtFU8OUR%2BbJTnz%2BqUO7gq0B72%2B5gTxmdIpWMHl%2B9bbNZLBBXm2Hhg6UstJNhPN7mz0vZc7xR8MABvmNLt7ArmZFH5nXndwEetzNjDInYGncZpK4X1Fw%2F%2BQ4g5WJk93KFjJBW0bYJEa4%2BLEMks0JFBnkfJwdnlqg27i%2Fps0uoSqb%2Bhll6AQ0v667VBIPqCOSRuaCxS%2FM0%2B81kdk2q937q8Mva7APomq1AwgCNi7S6%2B%2F%2FoqUTl9H6EHAS8xNLmFrDJ6%2B13OQukNSLQKzOYTtOGOM7YQJvsQGoZGnkyuWf9kRsjUaKdELlrbaq67BQnl5HdeMNkU0p5UIcGO7mfkwrPmjkVcU8eO%2B5q2dtgy%2FqITiDZQ0nK0hm6J1cZvhXc31kjjQf7Q7duajZYWBAy%2FrH0rCEIZga3nhTS419n6QNJsyL%2FJviUmYEBV0a356K4zZnpbmKhYvIw%2BAsM9lUg%2F%2BAhyjeQCV%2BpTEOXUrIwvr6RyQY6pgHI0S%2Bt%2BTNYwrLO2BakGF2pFbLnfLvi91pZUQWeNUqYAmF1O3Ts%2BbWlQGwdF%2BJi9JEhD97KLHz6haMQBihA4lDSzL%2FVF70AtsmEMsChs9PwSeeRrWEXLddQ763rmUDkCxMBNfFRIktcm9q0An2XfrQJ4FXhFO40hJ2CftZ55%2FGdjZmGkmcVJSD4J%2B6P4qnlu7RatzPzQA9jguyflGYYePVYHQKD81sm&X-Amz-Signature=6e7e35110a3d5b142bc3df9e192511d0340d83cb619c9bc4170f0959ca33ed9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

