---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLE2HJ22%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T070057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH%2FtVPW5OzXspprQBLZqnXZCZm0kiGDQ2gvhARZyZ8RRAiA8Nno%2BSUs7GEsiO%2FRRUnjJZQzpzLIDvazqao27QJ4F4CqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1CCF6wfgmRQPN9lJKtwDbgg2hhMRv0YBGoKLlDDtZJgtgxyeO2uwPCmSx9Ius1Js2uAP2Lw8ttZkewIKCn3eo5MMgoBOEueVGZm1xHqMcigcYJzuHUjgfFdVCns%2Bs%2BoQAua6V1wnMOJwiSLt3hZMtAGmfRXhixktnYkG4FOQAVB4rLgaoQT3DxpLFSU2Y2C8FaVEetEsArYapLQyG4fbLPqogi13AgTWW9sH%2B1xWzB8gzkENahf%2F8JZmooM61PQIjxlvQZ88LnwJlGGoVYsx%2F6ABv5v72vHswGKxOlTYZtRiTme27VvTzCMEMrwxXLxhg9NgV0j02z2djiLD7nnFwEouLlMyTQhGEoTf1V6oGOXWlru%2Bde%2B9wH6RGbxiF88n06k1FvzEaSDSOIXYblrzmmCD2gawCI3oRHWOcC612zR%2FT4IsADVfIUhB%2F5QTZ4fj2P7ncb9bSZB0c8FWrrwN%2BCyFqAljTwjdbL69gFTpunzgaz4tXdeegO0f97tW58NsIhYtsXona30Y%2BAmf8BYsnXK2ebP%2FnP4haE3vxa2sSFiQ0O0mvaj54YsL%2BPXnLU4rifitGHnDn4b2rbgse9ljQXgGu%2FCk9sYvXpWT%2BcDWLtKz3UlJEn3cZcvO%2FfnNckkP8DUbKLD%2FACNyRxww8aHwyAY6pgHqh2qK99L9cpEZvwzO2z7xuq%2FUAjQpPoOOs60SbpyzlAlcbimo73QAV81Z8kZ2oCK7wNVgPNgIOMv3BrDH0YxEZTcUbW3LUhPUqjQqqdsnv6SKjSix8y8hxxZYLWWOlUlI%2BfJPjl2chJ8pY%2BAo4CIx%2BjZeva88oycd7Q%2FiEhA23uKgh3Zdceae658Zzqb%2FvPyssjh9zVt54u5kY51OvOFeoO%2BMwbfC&X-Amz-Signature=7a1b983b657e02d242493825759e3328270d25353f544ad829dc7be0c8525573&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

