---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667TBHOQJ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIB%2F2mNWTj38EaVSjKbdvPqIbY18qLQirr9%2B427U7DILgAiBiihVJqBqAT7G1ZieRKfwPyYnvJgksze2Ax%2Be7fp1kdyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXFWQ%2FQjqGh9VnFiNKtwDFEvmJ3Rxaw013suhta79DOOC1IXkaLpNId4AaYa9jxY4jiZXYWQbQLRztZGSYX7vgWX4BY5eZj%2FW65xX7sYzM8SXA6XUG8OGKyCbnFArpiMKsWmZ5CoflJ6i27t1lNGmoTsoe70Sab%2BpUc52FEaSkL7IXXWvf3AZN5Da2dHfEcta60kMWzr00vRO2bFBrbhZEeK8nxywPJsGPtJmTHeXZyPqFYztHVtay47bHuNbkXTZGvANZoAXwiwlgcHZAxZkjKL5aJkrggAZTsAGpBQE%2BDtXX8WfjLAWDdHxBhk6BJWXKMHWdTZKyqk9GypLboFXO3VSslro3ptB%2FRO0qOutV2fOtxuWLqp1Xlhc6hxNd6gUSB12Tq8uFgGhkHTr%2FcVgOZF2AVNd7arKqTO4Oj3vg1Sau4dc8TS3ER6kEmzn2XBLReYHUu%2B2aCwTjvV3EIkgNC1UvTO2P8%2FXKJfV2yb%2BkBQWbjo3YVXFvWm%2Fwn%2F5nGoTtn3BFqocHs%2BCuRiv32i1%2FFWGCHNo2UEM%2BnEBerj7Ev7nxaHh%2F5IWlPhuMdf9edXoclXJD7fI8gza29fOQNfFhGnKFZKVMeCisVeD%2B%2FtOn8lEqcOaMhKz4kCuevySaHUtqoIbzBswYP63Nv8wsofOxwY6pgGuUIJnnl43lAJDrJl%2Ff5CB2I%2FeTtdczNRFcrsKwxRmxW2qpiyluSKqDNwXcckAYBFLr5BUeRuLebj9SBG%2FNhVqscjiqr1JWk7v5cDpsI81r4f424M2p%2FzYN0Zd9v0WOUgx5AS1fjbgEUlqV2fSxTkAWPzRglwvn8XkonTTBwqX3zdPIuErwRBmojpW0aB2f%2F94XlbJwIs33g8rvTiihE4tRFimbQNQ&X-Amz-Signature=6005c3ff9dcc7daf70ea3d52f9ac755e68e869306d9b5f70c00db1a28235ecad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

