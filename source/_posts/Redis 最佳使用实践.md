---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TC3ILVIC%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5mqs64MtYoWqb66D5VLkTNkL7SeK9iYZalYGiwKoNkQIhAIC78O4eBW7%2FnQ6vo9izuzE14VHes4cC2L3DxU8tio0hKv8DCFYQABoMNjM3NDIzMTgzODA1Igzl9gwNARluqUp9xW0q3ANySzkFztfcJmJWTTvOxGLahrwSe8wRJyXTVFD0BPPK7YyN%2Fgoa1Q7k%2F0Qu1T5L16AFfVUtA6NsIuQnR%2BKA0MbznJu%2FyrwpIepZTcNjefhilaZSklJ9bDlKl7tBMFIkXbhKLTS8plABYm1zZ8rxFk9qxdYMlHY4i7Aroaw7UXAr4Yyvb15SqcFljpFLnu2yQYnF0%2FXAE7qLu8ICYSmoKF1FJ6BTFK1ap4SK6FuOx9ewEZUbcY2lBLmpQ7GJ%2BUcjCN6xtKVIh%2BxtIkfiXqzLxzWKdv4BUQ7H0bzm%2FWjT5K%2F14TsW1GKn2vsgvUtRwNDw4JvwEJG9KlxV%2BhGJEjN2KSG5%2B8CqWPbRA%2FgnG8Dt25e0jQY1jXONnZKPW3ghgrE2%2FG2nD4eFjzCoUlU8z%2BBAckSA21xl%2FHQaL5pU%2FRT66K6gxHMS08Ek8k0xU1TV1DGcCHlM3HA%2FtN9HrCzvDp055pjFGhscZeOTq3Y3IpvDNLLv7s36TN7cX1GDGkVymOMuMbrkTFilDVFdHN%2FMAaXV1%2B2rd%2FRKNxvm92Ep1tUqyNVhYSa59NhLUDy9qMHvqvSdzW5j5fzJk%2F%2B8lQD5%2FQAv%2BSXVcqjvEXEap6Xlg7JVcLXpPGIhoj40bHQ%2FmUaRLjDa6KDIBjqkAcVoImFNreSGYjrGqTVuqmJ7FSFwQf2czd4qhLP6LFr8tduAXwr9pLx3Va%2BNfY0eMK6MvZPnCnR7Hgy17%2BTKp5TpKy%2F0mFZXC9n1W8CLE8K2yKTLqWKXaXO6k2VWwj2C9wTXldZQT5UcN2GPkEVqSyHGwcgFXQSbKU4xzh5SXwZPCI%2FFlGl%2FVHRHuUyb4eB53icfTKl8EvpVfmxnfdF8PSByffB9&X-Amz-Signature=f95e942242ebd1e729489fcf86c58470e23155de3bfb9e792236225f9f35dd3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

