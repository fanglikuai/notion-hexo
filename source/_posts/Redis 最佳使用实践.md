---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EG2RERP%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCICvllAKGRtT%2F%2BUmWIpIgoohKfTKhH2M23WwgDrb1jcJRAiEA18Lo0y6As9u96cqpXEV7JTx9HOYab3vwqMQvrma0n6kqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOnFN7IQ4Hu15XGuircA%2FRcw5wkqepkZFY4U6pyvobCd4ClLSv2Jhl3igQpNfXKMkCiq6yzrdKIy7J4sufYoznKmoRyMJj0dJ8jobAMoOR2m1sNAvyERVGbwGfm5FQqGI%2FxlLNxzc6FAvvgm7SJj5tiQDXjLIWkZu8qOHs6AkApW5X2hWrzmlBhzEl07DSMTVWXtd1EnF3cfvYfa9DTOQ8TLJ3JAQMaBBA43TWCu8DNgkfxf9eLaBa7joPKHzkvvWYRC8VW5pIvSiu6cyE%2F%2FGB%2BuozAFpps9gNhXIihnPouK1cgBZ4JxrkrE43En5AoliayQgjuudZA4Tp0NbFezFUgcgmnv7lhWOdx3bdcEqpgcHe5kyT%2F2ShNZkpWDpJHlxaPDtA9vgL1%2FL4fm%2F3e1y4ycoQmvnzrleaxPn7S8m6jwySbCvCI%2BnxHRYSyNcwN%2B4sHskj4R%2BrWqvJXfwAkfT5LpF4orlcKYUDyocIyDMZWfTb5kZ16zoJ41C9oCRmyUguDxqSV2Y%2FnCjzqQvxhzQ5F5SAbgCLjyUSRREf0nMqywi6KlLBGhVLZWSgOzcGz%2BL9YA9P6LeXzfMm9cuKw1950Ju8RoAGgNKKkRxs2FVVOhVdAeJBJeoac0%2FUFR%2B9ZL7Vn5wlRwmBxNQZVMI6djsgGOqUBBr1hUdW3RpttE9oytJhgN48D83UG%2Ff7QFziafkRzQ2a%2FE0x7HVMh5MjJVnhHRBt58QT6GqwmCoX%2FLRHxVQIADbkwXnwWyvp%2F7ze6qJ7EE6Vl%2BbyIuBKM8sSGLzgisghBy6YuJNZSfz3CMrnjk2DY8OmyE08xCxx4gClyAvenLF1epglfxGdejC2kBJ1uEmBvs9I%2BVAEjWyjcrkgqyIQqCBCRm7Cz&X-Amz-Signature=09b85a2a0279ed77188f60fdc26853e5eac6b8b004330bcf30696fca240f76b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

