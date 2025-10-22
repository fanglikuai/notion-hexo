---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QOO4YDR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCW8%2BSDHK6VPW%2BTt7c2hDkqvsxEZgJotc3cT22%2BfzvMrAIga6MJqOqHfyo0JvoiaaHSNuhlJXPtWrExbpI7X7PeP5Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDPmrXfMer22NRGpmByrcAxM74MabOKtFANVti6xJeCcko57amjW%2BoRh0IpvzVvcQAbaSSKfa%2F%2BrmvZf2hPKdG%2Bx240R%2FZ57QcpZg4RZOEgAZOMDGDv1MT7PETdBkmo5LLKU7lipxiJuqMfY49RsjPA1UCSLtszO%2BpuLUNX7keUjdh3VvnXKgBBtVsUHtloqTTg1ZVgkvAxsDh0Elj0eyVGPkI3O%2Byd%2BjLU3COfGGBLy09%2FIOiJFEUuEq2K2zErUpsmyGaF8OKtXfbw%2B%2F6elqKrJawuJSWpwl4C4R88ba0Od6NcdQVu%2BcC9evgLZuPAge0gFPfXhhCDaDCIzYDyutGB2ysDtuba9PPI%2F4wh5DEImMe%2Bmu7F%2B3MPeT5mWEYqd3dQJMFcYeJt7Kd4iNJaIc7WgEkM5Fv4FTOdBX851B%2FAv1afnKC5muA1TqJ%2B48f0KOxYsznKLrGxSebo4pEN0nD9lIHTbre59t57bBcg%2BVmERcvmn9Bo0eYRQfecd5Uf%2FjVfSIs9%2FYdH%2FUgwi4MLx%2Bt8tlU2Y0kEyZ51E7Wn%2FrLVdlKrBLyRGLdxDru5fI8fVjFFAqxv7YJ4Mi7Mw5cx46ZooTv2fZ21YfPjt%2Fc%2BiOnMY9lPHPKaQhs7wE3go8kF2aeMB5ru7o8Pk%2FOEOBMMm65ccGOqUBLbcCtN0%2F0%2BUa5HzZWbFJJbtXbcf4ahhda4VZy%2BiGjAzu7XqDU2w4QpUmVdmlPVAHP09RKzPIvw%2Fj5w3ZzJJqFl30Aq6MAer44SyOwslvoSRtrHcSmCoS8utbLk1HvlZAGTf43GVRM3VAd8GBENdT%2FGHjflF%2Bv%2BRC48Zrr0l5QGUYAhzJwtkRcWfS2TKqRA%2FBTxNewons2OB5jSpEPM2dXzI4esw7&X-Amz-Signature=c62abbe46f97a1d2bffb79725a9265872bcefcccfce6f53d4678471684577350&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

