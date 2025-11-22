---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLY727CG%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIANE5x%2FpCTwPlaktxD%2FSpMUaPal65Bt5p7yLU7MjcT7QAiBpOQ4sRtJ92GwN9JpfnzyvBZzWQ1sP9Ek5ojwim0fyoir%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIM60lcRx39CwZUyu9WKtwDW5elvCMtKXIudz0CudxRAt9VXtKXba3fXHLj1XhEUr7j5H%2F7pAUfwdy2onFoaz40d8Y7iuxk%2B7G49lh3UhSaCFcbZhn%2B6rSUMf8n4OQd5DBgnunSrTOlNdWAFXeV5lkyQqsJTqgNwmk8gZJROft9d5SujukaAHa%2Fk4s1xYP%2FUUTd7EKUcXao1XIogaFmX7sQMzt05socbgz0D48a0%2F19Yu2kDMvsCC7jjrSulNsuej8LlnXWuqi8NrsU1aS49fa5i8KimsFVFfkXB4tOxeBH03l7lW88g4uQOW0y67JH6icdjjDv6ywxfTtxhQywbM1DBay%2FwSsInFXSeMFrvARpLi3s8b8gJaTdQN%2FUXMeoTyky2yhVuR3v0I3ERWScF8FBPGNdz9Imxt%2BV%2F3aw9iaSiKIKc%2FqYBbXWGNpTIfksplJLflGf%2FoFYPglCZlj3dVFzYVaiBGwuNPF%2FEDFR6zck22ICzxJLA4YYbGzZpb9yqUcXwUfhuo54yM4kwEo9pIvXcBr2yE0sWX8bctipzOOXKYejfGuv3muNPHzQEoQoTwsU%2FzFJlYSmZK%2FUFDf5GCsok70EWhcniouNs0qSsZtTuy8HjjpFz5xHfcY6hX%2BvIloT%2BuWVNBrfW5IzPb4wtYOFyQY6pgFwqszT6rwc%2BO0or%2B5GXxgmDh3AX2l5mHph%2Ff4WxnXDquJorpk84pfVayULbVSzSYofrQeAlUbTgbi8b9QHtKZMQlzY%2FDB%2BZ8DLonUhguZoqGoRbabVQMlCxCRo1jdpIXr2FmjrS7n02uNPfMF%2BnPXaOrhKw0Uaq25Kt%2FPEO6v2lX7F06M8eLBXK7NmK0ZB%2FmaNERZx%2BunXQuawbgpE3C1ZDxYB%2BBVl&X-Amz-Signature=c8d5e341a050ab51d5d652f70b1bb31c39edcfbc6f75a6ca15a1666e605a9060&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

