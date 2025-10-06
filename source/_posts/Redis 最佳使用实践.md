---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOV26MXE%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA0j5wp8aHoIjO54INl492w2EPuCkWu%2F6baM4iXreUH3AiBcxII1LFrxgOTo75%2FTwWKy35n9P%2B%2Fhi%2Blph9VfD5thMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNQfPYMfv%2Bj%2Bq0TKoKtwDAw7xGOK11FLixiZKqhuLPyC7eI7M1bs3I3qO3LQBQFnY1HjDGcFHFwQ5igKaNpVY%2FhHos0HhfyO5VH9FUa0ByWL9ZDSxdnSh3o%2FMqng1NoEvptVa0qgpZXF2dvjzVp%2Fu%2FHMRRNcyFUHr0gHdHFneWF6Hl0vc6ktXlte0wQTIxyQBAWYy2kCFqPt%2BUB%2FbZ%2FXMw36LDG%2BTKWmKJivBb8ta%2FtzNZyu93KYQe1stOMRMBygt2CAvlKNk501m46CRCyLPg9CxqjmqU3t5%2FhdnOAHoCMBThX1%2FOHwkobQ9zSdeE76LojutGJYoh83lqZpOS1LWVuNyUpwUrVZS%2B7qjO4toQf2A6dPi5gY%2FWccGpAvIWzt%2Fy4%2BlfM6HDuWivlxji1ex3gwT4Hp4t0jZlYerNr8Yedv3gzKqZx53KIDT0oGmAR3s3I9AX2UaPDwyPvnqbZv%2FPseeZ%2FgzAvXLOIACIBP3wuJQJCk1JNAgZqlgH3LlKUwjpynMALSlaDOZOK3qkif%2Fb0kN70B2Cjm2p8bfkz1fi6RxOH49sjWqbV%2B%2Fx%2BxX%2BTjquKbNIGCFnjLgl%2B%2FGA9zZ70jwu%2BSVSdu74IPo5hZNKtqHjGXv2XzSgSMtKWebvH%2FnWXjTrC%2FKoV8lS9Mw99CPxwY6pgGOIoLkNkOPXBkqc42WsrtaWsMEwILsrm70SVsmYW2KPYm7fsX9iFFlYkW3CErdVpZNJ0uSlIWfITeO2%2BjPMQNUErScAQaxeH9vNWzXYyBB8mclGJA3nU3rCUHaXym9lI3HigDr0WcU%2FfOU7SiEvxl%2Fl%2FdVRAodtxpMBWg308AWpwGZJF1n6AeVXVV4Vf68r%2BQkmtIw1phPrKaDNP44X798laQ2ymby&X-Amz-Signature=cd998a2a11c763a8a8be9a3f00e7f4df062911261ff96bbc615eed366b7a4507&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

