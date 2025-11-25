---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZV3OIQ7%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCH4Jm%2BnO8GG%2FBdYGcOqP157qxASYIPnS%2FioeLlMVdlAIgD%2BO5G8kq3ALZFyFWcqyzEfKVbs1YXC6NFYCI4qc1Kcgq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDHi3jmaZkzjt6C7xRSrcA13aeMft2vE91%2FyMtqd%2B6GHpLu06OKxUroVk%2FZD63UQJqCw%2FOpXPT6oPHSqh4nhqRg1DXDyg%2BaNlkhLs60%2BNgY2IVJRXIrDthdyAGCmS7oaJLMQXvxEl7cmev0aQCdvFPXlQb0lFn9pYrSDJnbbgeRwFnZy0e0RrS8n8sihX8dwGobgki7HZ%2B7h8wxB4itQL5HPU75KxX2guIId4HoBLn4BsmIdpnnFM%2BII5wkPlklhQSZsKXY6xWnjUoRYJsf4xDahGXGIOkdSfxcgU21LBxrQOUm1OLBQIt%2B2l3QhlO%2FXtiSLJK8w1rN00LUxPFjnFMLcr6ZJbLgpFISm8J0CtezKZuBTN2aS4FDdFzKEgdcIT8KAUkdnp1asH4MFDGoSnHqCBjErdRAO6iCap4YCuHSL%2F7rX4RNtdRSHXFF6eqvPdQgdpibBXwj10nPQ2NocoBvmBDbCmy0hVDd7E7CMA8l23v9KKrM%2Feutwypta8RveDrsh0Ii5pJJ3pRazjWDJvQno29CY%2BLwaSO2DTde03Xe8%2BzUTT%2BXIbJrsmoVULrEaxlFSGi4A9K5wfC%2BdnjJ5vOATgR%2B3VIluF3XnvHPw6fAkh76106E6Tt5eE1YRQN8ynRqAY7rrwcSUT3VD3MI26lckGOqUBeIvGnhrI3MkocKoKQMEHR4Izd7xi56cjKbIG3pB8QQg0hmMihBk39qzH6NVNBD5IzfZXrV6P7fy9nzeS8zj%2FUR1fWaw9fzscnJ33lflf5PhWZ6HQ8YHbvUC%2B3xIKCtdV1N4IbAbqkvTSDaobJ7tQTXnlfkFNmUKNerUE7cMZP6Y3jpXVAVwKh3F%2FjeJXTYNEB%2FYsLadA9NU2sdkmGm53ZJqi%2F0dr&X-Amz-Signature=213089fe1a6c7686ef5d5945435b473034cfcfdc54cc456d2f834b56f4206b79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

