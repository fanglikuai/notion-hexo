---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673NX6475%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T160059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFrOp1Mf78ctew1oIpSkWJwJiamwt8Hka%2B7X51QmY2kqAiAY9IucjdXF0EY5Ozpa%2BVxdBEXBCnxEZtch3QEn%2F3g9TCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMUp%2Fni9%2F%2B7UEq0DEKKtwDWWi0Y2KkxGbUHCvTN%2Bi4eUkfRnUWIrC%2F3OrXwVojlhcVQ7F5B0I7UKANQia3AVH6cYzkqIvoBFjmav7YK9AO9DYilbsvVqWoO41Qy5W0wKgWA9Tmaa3mM7RRZ%2BRXWg2%2B6x8mIH9a%2Fy%2BMe6lHBEv8FMgK4IL%2BZHBs2ecW5pC03p0%2F6OXQygEAFUoIcyLkQT7ySaVOLx%2Fd1JrBuM7qyCJ2lfDYgSZdXfCL%2FGFuqwH5X4nx6qZ64ANEk8V06eOb687m5bgvSoxi5vDKdQCemSzF7fszfqGkc%2BLTDDtVrnasK8QelFqoM%2BQp6jccItIc%2Beg9uPRxPsLF2tz8IWulVJCfovMj6Lar6TuYn6TaTFr%2F39M3zbci1XCKOUrqcBJZeMFoKZ4RU3%2F3pK2EV%2FIaiLAcCifdYkRtGFevmmyGdB%2Buhsz2zSJtDW46PJaeQSWy1zcyEBmhidc3bjFhB2vJhp8qk4T8mDqqHdfCpWac%2BurPPT5ea9EuPZxaHa1He5IDnMAl3s76ggYMqyPRb0M%2Ftrw6kOPY2p2orFtqmVdRjcDnGyVYfLhaxpKrrNsHNnTk8dbyELnYByLUCpQjpl2l6NquLN9eA9F2wjgEIcQjP8UZrqLYdSmkixDKHvUudfEw7pzpxwY6pgH2JJ6iWR6f0OkGqmGmhRElZkAkpRSgydge1G4DQNQk9Ph7DjjibKRIw4006rlGADbXoGsVRzZw69AaewkiGBy%2BEaDZScj6XdfMFFSMmwUdxn%2FlTxdTTO4nMFKB7qIU3oq%2F5VaEM8mQbSw2dluJ25ccUdAoS%2FNDMURkwvfAuB1caF0aNdH3hJGhfuwd9T6mM5DN4IpAGYcR98xO3S2uZ%2BotF6oYkdi0&X-Amz-Signature=a82fd1e4ae593509ec3846dd62fb8a646cc863f23d344525b9753e2ba4011d8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

