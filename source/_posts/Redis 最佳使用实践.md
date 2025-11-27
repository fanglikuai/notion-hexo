---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDKPSLII%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICzwbPfHJMbgexgiBJyFvrtdAeadPlvmP8sQYfeWjUaVAiBfg17RwMEgZvEr%2FZHq7NxAyRRfOQ9ObsugsMg7E07QiCqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjkk42f8LP18mC5fYKtwDhtsYSe5%2BI7777XQMRQPQJ4UaTYLDV6DvuNvHnYarROuGe2uI2Elk%2F%2Bq4smb3NatSq8mRfvKwmfwk0BTFSjygyT4lGf1IRETXwmI899m5K8Y7vcFFRpnt%2BGCRdu5hz6%2FaM8p1E9KamjU1xVyKiRIgG1nakEuPPJIQkEzBdX%2F3PsU2fIE1omTtFw%2FyZtwcLvXlhBB5vM%2BELJH5%2BWOBqFUdgUMASx7AJgI301IsO4YhwFemBzG7bzW3QrfGhOTacjr2HIQFJrecYLf7VR2MzY0tXhyISYaJHytoiyjs%2FvsGHY0iCiKQk4lD9iTtqIPucjAkAouUD%2FQTelWR%2FKp3xi7GaZZjBQH8PZ9VHJM%2BW%2FdqHDeqo75%2FWS1K9VBN4tJRR5%2BcTxfxUEfPvWQHJaX1vsgWNrHMfIIGHwEiVSks7tU0C68GhwAwBgwP1ouH8Y9u1XI0ci1cEAIVxqbArKtguYHUVKgfcWrU3i3AJkYEvdHO1AlFZ8MIPfMj0HAtxciKNGbWXng03mHz%2BmK7Qi7HD4SURXij%2BVSiIA4CaLxZ0RBa1abSlIeiYCWfRZsiQMW8azcv%2B8fSpqhjd5U%2BkQ9I0vujSiJjawF9lEHwGTEl1lHqOHOfJDGHfmtTqSNar2UwkLmeyQY6pgGsaVZ0C%2B8XdwDV%2BF4qNcu1fVwd5YzAl5130NefHEeKCf57CHHo0onNv95vXyFTzBS3ueTCQBwwb%2BVfkQ%2FlBNFury6QXkmD0t5ECbWSrtcM26HJxE5M39PfyhnfMO8z54MdySKkG8sd0jLpAPBRB67oKKNsio4aULO%2BnrYZa489%2BUe%2BguPstyfTNXkQxCGT9LVO9VmTbxhDP3IK%2BGRG7LZQVGeitzG7&X-Amz-Signature=88d4efb64d032f845d1a5aef156638908156fbbaf0dbff0d3b3214d88ea58ba1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

