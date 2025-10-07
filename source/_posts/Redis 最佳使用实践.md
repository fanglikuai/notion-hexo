---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7B7ILP2%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T150105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIB68Z9jlxH%2BnCbCoaEtBL2GctrKiDfumKUKh0%2Fqvd8PpAiEAvQi5vsCMM7H%2BotEYXHx8VNn67KfNBrynaeHIQR%2BbiI4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDn0EaKNOvkLOP7O4yrcA1rvA8OPzXm6ch3jWmgsNtAqzOsSfLFDl%2BmVIV4uIjRa1sEHszhi5Yskbho9XxxGpsI%2BCIFl%2FRcKnRC5wn2nX34O6I9OZeVfIdsGtyKbTa7t7gLvJLi5C0TYRUlfBrBJYOa6L6xaidXn3m1O0owBQBrv8FksAceuT2oJCCAmJWLxpzz8upUXdJQ%2B1qH4ZZqZhIFy9Pr%2BOop4T8C9rpihcyAbG2sPvCsNUn4tb0PusmpW3SEU65kBsjTvSkccauw2PsNHUXt4MZkSxyVMulHdeOOuqCPiTBStPKjoJrhVfhe2fcYzDKKuL4z%2BLK%2Bl1qWNDk7u2pTHHtWucW6U%2FXGVVuUdCp9lDKuHnaIUBtAaE72dfs2y1n%2BFPWiCB4cgFU96gcrG%2BJhFxPlf%2B4zPSJpDa23BZ0eyqvuqQ%2FdBfu8ZfFP%2FM2I5cDwtU5boZzSLJpivEvpi7Kysp6KTHFmimPHhpUsXwUya%2Fm6bsMN4BB2pVJI5CZBNyDZCoVyl5%2B5DWaIXp%2FkrFNmowPd4OqGVI5HpeBxImAGdxb8nrnJo2%2FeF046uJLYE8kp6S%2BbDuVXlksUQuySZ7bkBViwWKZSH1YnUPoYR4SPNvUEd8x%2FcWPHddKTKFE90HdnHMkCYlCjxMOe%2FlMcGOqUBbEmfdkD8pa1fJLBWq5ZLnojjGs%2Bd6OknhyiDYyZrJ2eRTVNJ9mIWidgyrES2RvBtoKXnk4MqDKadQsjXh2%2Fj83tKp6DG3jqrsWyEDMyXhQFpKX9sJan6z8tTWWPU0tU7mibcOsg95lsMpnYhQyWM11u05A23VOD71ZAkX0Xp9UITPp2i4e6oTATTrynY%2BvOhOOYbrCTrs8LlxenONg%2FZvxr08L5y&X-Amz-Signature=862034b84cda8019a30a4e093cdd4edb14e378a650629ee881789da6e4348c69&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

