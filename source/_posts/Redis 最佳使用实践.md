---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DN4334C%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQCe8uOkl9w0eBbCW62QUilxsj%2FwoJwTGYzvTpPLb5OXRgIhAMfhbowZP3u3AX%2F9XS%2FdPQCfdgWFhrC3av4f%2BK0GGQdaKv8DCCIQABoMNjM3NDIzMTgzODA1Igy2T4yUZbyfzeSTIwkq3AOA%2FOGmqvuwJk0QIKo0iXvN%2F7eaeW9KrcjSL8XgJT5458zgpCM55gR8Lgwas%2B3C2eOoYocaAVy148011RZA6V43vUwYyphYBWmRp1plDUTe5zqvhUqlmrDeu0MxiroCF4MslgU05ipyUwmDJjuHIyhvwOQZEXJXJxS5Owc%2BfUxu9WmQ2v7qdENt1KO5p7Vt8xCI4i2fYcLPy0VJB%2Fp3qRx%2BS4ssaBJrUD4N49VhAPTpTj8NZVSWIzOUfpOs4O8pd7pvwgYUcyYvhOErZy6YSTRunBYRl24rO7AD%2FsmuZe5Myc6bi5eUrOg6mQwOKKSdKOHC4sHOKZw5bzBxyrPXvz1Ejd1cskh3OFm9DXCoZMWMKmKPqYdOIyhqltA8YnMPqzYTa6qtMJBNBbcv8yFc2LweqBSHchoYmUR6N6RFLHDlWhAtz%2BVI4uQI3IgReUeYJi%2Ba6qzqSqpHrVWemn9%2FUKPSf%2BdcTGt1RRbgMwnamgCkShn23h9Y2HiooRWBH1xnjMx%2FYO20S7PuzwFOmQoty%2FK5ApJk9wlJfMAuSJpGHiWcA4yucenW4dNKtdGH6ZrVgcLCjYK0oYvko7vaDmKQINftBLC9%2BUWQaaLXlhNzblnXQo7OR8MQ5BlA%2B%2BWXMzDp383IBjqkAa2hxSmzJWiAhMgCEsiKqxET1E9VBGgYZgmX2gI7vQWDjoa6REOul5SR6SSFxB9UidiXx8g%2FxfYT8ZPFNLUyjLvH2jTXvgqm6uxYvxxVdUB29N7JI3EUNWkQQNlRDTvMqsqACe9YmjFLh1F7j9S6261XYChLlE0KhAp270fZztwjw962bjzFqo4pICyvz6%2FDOX7zYWt3NLixATWaTCCkQWywvoFI&X-Amz-Signature=4446f6ebdc0ed847cba8d3caf5df2f3d7969faff8d6df694333459d9678cce1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

