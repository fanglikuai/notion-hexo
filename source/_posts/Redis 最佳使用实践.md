---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNPQCPLK%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T140117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJHMEUCIFfBeOt4gU8wL3TEY7MtyF3Z1FMwrwT4zqykjXEpd7D2AiEAjI0YwN0uMb9awZs3ZhTVjBGGtVTTvPisecyB3albpFsq%2FwMIDxAAGgw2Mzc0MjMxODM4MDUiDMPpE9Lo6Y5NywoUXircA6kXaTCFd9E0EX47Uz7dn6va%2F7A%2B5PzhMxrkfN34Q91ZPAgHeB9cMJA6XaqWm7u5qO48oFQttiIAnGsA3FI%2BpSRvq7Pl%2F3aBhCBVM3SK87SjE9DuOh5HLKB94eKvEnJYh4BYuGHUjMxYG1aeebbgNXqKYHKQLmAj1YFqua%2BSI3dxRPoRrQw1%2Bn6zigd801I%2B4B6YLDN2xk4ty19xp9tXWya5K2%2Fj7LhTRbHZAWqpgayBSF4IWGrm39tPLk5rH%2B%2Bev5Y%2FTGuK6syaGNKEUqY9%2F33Rg1olNL9yfu%2FlfbD1yUp6NmTycLbkJ%2FwGSSrGuGyyOrdotXPhWn%2BUg3FoRILn66AM3Zq9XgRs%2BOS3pCH6VkWhbqHGCodcrWAdWPChAAX%2BMuzvSmCY5UyBAMikWYrRYwQBcNYdnERJFkiIIcB3rUZROqGUa2ZJGL23KsPb5G7WL1K%2BhzIcGTC6HNqiwKGaoEMnavxW9%2FHcbLKcK40VnqXE7%2BNGZTZYUE2toGPeXEFCbj7u2hSkmOefV0SzRarYjCaFZiLHuexvYaK0606jDML3SbGOHQhzY1xFQM%2FqyAz3Z3JsRfpiPBdzVrP36jtIDRjXkCov3q5dg2pSlkCq66mNDQvwb%2FsE0Tzgb078MM3QgckGOqUBWG2KtFI2LepEgnVl2hIv3oZgJwWBmzrOO3%2Fl1axqk9UApwVamrFCnWXnXktrBCPb04b3IhAUl8kkyYvR5rE%2BC1PambVqXxcn%2BNF5VyMXkasYwgSyV0Gf%2B3FnqQYToVLVDw9Q2fJJ9olJ9Nw1e8hYIBCL9AboYSv7KnLca%2FE3SRYJLbFoiRN05FY%2FeZgXe6Z%2BaIQWiU6LtGIgFINXWiA6bbpCU3oG&X-Amz-Signature=6101ec5409ba885de4b575c7879d62f2f34cbf85563b35abfbde666df6121ffa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

