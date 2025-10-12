---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCA5TEWS%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIQCLtTghYexVFTC0GKT2twnI5wnrJnR%2Bh82f7ol9r9XWPAIgZwrQCPJh1y%2FyUUSsX6Gi96pebCj0zO52al%2Bmgoh5zMoq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDD4SmOmzoXdPUmaurircA4paVpLbjEN9iMXO12kKYaxdGDOOKZWgtPpu5OjiUlEeVS%2B3Gkssut4FtPVXlKQ%2FGEua7RPLbWu3zZ5%2FKGe66Cnj3jH3I7d7T8ofAOmvqhFesNHrAPDbD7WR2tir5%2BBwFS%2F1y0ZEC%2BLWParSBrzyEJl4aXECzpRFNVzIW%2Fy0X4OV3PRwf5EmOjvShuGUDezu1UQ3Pzsp5N01jnb78NvlQ3MQOx2gzbmrXCB4Cw0m2jzro%2FNNq0r3rWzhRgU%2Bosg9OgvxJmmDP84pwl%2BpKr0hNoftqjPVeN%2BUhm%2BzSWOhJhTXc4jn3Xbws%2BTvxEnZ51lXQDkI4Vl2MVA0%2BlqNXJLxTZ%2BlPkudLqOV0pgcPu89xHxN%2FqRhu6j3nNRHybp2NQHaOtistLNpAAzCk0zSE66qAdPcIx76iXp9TRcndvAELsCs%2BqZHglqr8DThpGBKhj0sSnnWg7qOg0ERIrBR3JV8j9obo0FsEyvW4qOEVDJqzgiRWBfJWvB8SMHjgvl9I%2B1PvxtM%2B1FABfVVXVe4cLxZWBQR6bC39C%2BYLk9GdhtSmcU175yU9usgobYjlBSL6gP2BioShA9IceZeTr6cwhpLnCDqo6WAITmj64l0A7GoVEbDZ6Zyxjr0sKuWOHHdMNTDrMcGOqUBeoHJ0HaXoirPvIFar7XuZkWrqu10L7DiVUGA6JfmIi6mxYa4IJXyxb1BU31cpzzA3CkpNr%2F7Z4gCSuOanN4KAtzwTKmeHTvMgTGejBE16%2BrfbTN%2FPzB0%2F%2FgMwC%2FzS6HAXVq4VgnYyjR4rlsHjwX1%2Fu%2B6KQQvqmUPP3vIU6vLvwj9ISQvVdBe64B%2ByDT13rCui3H6eBvAl773R%2FWy%2FIQwAAFZkybQ&X-Amz-Signature=8c82c1d8ac5671dfa91b792f54dc98575f1cb7f18492e7b2d7403a014076d379&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

