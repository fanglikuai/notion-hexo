---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FAESRK4%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T150049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFI%2F6t2vPg5RqOq2c7QNXI6EPLGVonXCUB42Z6U1dqCIAiAIop6GkAl6PWkVUO9C5SGCUuhLK6umOCVTqetRZc5I3yqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTU1m1CDtWP1mFkRKtwD8tUP8Pq7FYbhm31vtkaH9Qs4NWEN7kXFT4uK35iIy4VC%2FBnc1PhK3u7pOMReE%2F1nFccngEMocMhDoSM%2Bhx54heQxmDajerGjH0WOaVWtlhRoN5e6%2BIloXCvOP2bdLFYkoNecIqv5Fx8GLwOOXRVkiF9WLYIQbGrkq8%2F6UR1MdHNArHn7ek3k%2FfhqMz%2Fs%2FJoov7TUJm0yop3a1Bg2mSgpgh1LsuIvDHxqReC2SvxBqDSwxOzosIJWeipZ7X08VZyYKdE58979iMJ%2BABHez6%2BqzUlI865SId6q3feTHv9Jv8otkC%2B6AzqH9%2B%2BMcln%2BImc0SReNFZIJD6wywLf4bTCoDdv6U9wqKNIlJ6PA4yE5ziSXJJlj7XhTcqZfSGicswyX0ORjvj%2BVywHGlKh46o6eLcC0%2BAYNmD37okTUh9Iu3WaQo7WFkJh4uZZJYEgcjwEBuexeedHFeBH1X%2B5tvn0mKAOVpCuiQYPhDTf6n%2FkssfbdHZMmmb0xuEmjEMkSAx4zxHWXVR8pdamr8V20Oyc5mPk%2F%2BiO%2FixRtrsBvepgHjiHAeuFg3ZV%2BAaJBFEptGnaSTxrq7Hpg3QnDDFvFCayeFdm9v%2FiAVcLVd2629SUvkn%2BA%2F9ckbfN5Tlzzdrgwm6LiyAY6pgESYmSTzsDQKXSRI%2FesuG7sNTdPrrqena18pCN%2BqjB0OdBRjjfOt3oRQVIQ%2F%2FRmlzmTHYlUlr8P8gKo%2Bgs0Sy5t%2FbyuPdEXnNdB501rlNAByQRw3Ly1lC86DAAY39Y1VGzCjri52T3fLm96YMpfv%2Bhm2aZjOzJNEHJSsOl8GCDG9JUcOlZoyJcGCVIvKWsS1vO8jALnAzLE1RJg5UP%2B1tRW1gDLtT8J&X-Amz-Signature=5913822c27e938bf4d8b221ce16fa61e2eeff96c30994843c2f023f649e9f788&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

