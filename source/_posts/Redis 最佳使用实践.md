---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RKJGPLYW%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T190120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCgLgICgXbLXdNOfN8x2LkPsHNwVnvhUVKzpWGOOciNkAIgdcU0rVrP2ARnDiIqBXy%2Bi5vioWdEnYUuEToghAIcO5cq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDCINwoOZ%2BTpKl8xovircAwg5La4Vh7igvfF2sohx8Cu4xeMmLPnAzOEww6S%2Bwe4ACM7KuLITi8tDoUzBQj6A%2BUTJOYcp9jme9mf7xHCuEIATlc%2FuCPnM4TGMTidJp%2FCsFfR4PGZSrSYY6L2n3RurICXIcVwtC3UtzEucGhQLsAQOtlHXmZZMfOJBhG0rqt25wa%2BDlp4teIvJDi9CjCCYwN0VaI3V0wIsFDFh8WmQ2RcTcByYBI4IN0OoT9H2%2Fv1pDoBlps9YfJilX5gAYoaAuHRQOHPBPMOGlPCm0aHMhvHfMpzwS2nHSVSa6bBKIOQBu5FRhO%2BKGAHKLDk8KxiZGK%2B1fNEWsypzUqGei%2FhE6PeqqOqVt7eJnXjgAOS5oUDttJnzMXPsymCP%2F%2B7zJ85dp%2FPcebQlku5jjIqgiMGcBDC3LQQ87BrMUgUAVkP6uUmWfiT6PDWfU5QQGAGbd1tMro%2FVanSR3sdVv13j2eNYPF4YTBBXufzMHqyWxEIGduZEjUT2uVQ0pNtIVccmKjVJXpIL2fRE8xuXEzMrhN%2FDS2KoEnuROXGKfDQzOACCTrr3PPTDNfyhkonYNmUq%2F58hTKIIhG4%2FpVX8b9jUGBGHhErkMAuiUPmx4Xq6ZJ%2Bx1ie2kCzlONB9PMiujEvcMJvz9cYGOqUBwE7XcdF%2BM4XPrLgmwlazcOx11CbOxV%2Fh%2Fd%2BbT9xCB0lW4htNyteKQXv520E0X5eIY%2FXkRHOHgfl%2Fg6w3Nep%2BAGoIA%2BG2xmminhp4aJr%2Bm1rMMVR5YmWBTMiLrTARwTJouFf7ed9VLPhf0b5fEMKiIE9PILdqN8dA3r6JhWYwNHdI41GBn1Y9zpBiHUHiY6ggGG2u3P8kc2OWoJLjjXemncfq84mI&X-Amz-Signature=e87b961c772ad941e1b3b9faaa498a84fb18b0ff36b1f97ae6677f8389d4d187&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

