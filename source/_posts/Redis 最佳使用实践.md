---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624SGGBZQ%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHW2gMmLa2uyo7FbIHnzFfn1R9aAOiQENWilVTsDgr23AiBZik9JaHOkBnwWZDl9Txexy8vXuQqh%2F1rYq9tvtNkNJSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMuwcXRq6Whgs41m4GKtwDIRpRmYCmp%2FG7mFvB9AO38D3HDYmQ7f77l77zHdJ01HKYjbpNd93CsR9qeIOgBCyg1S%2Fe%2F1tlv2dqbWdWr%2F3JmOXT59gNfHTT%2BQ2%2FiyAH911VSOzSkg04yHOhWPg09lln0XaI9GPt%2B%2FeuMJUH8XEoHdQkcsp2ZxJD21oWIKfO2OMB0fqdin5%2FLw7N9Gyw7xTF46cP8gWlVcrAS2pFyxXxTfutUda2Ac9S8VznFOvWzNKSqPIYKZEEFbUNRFf2GL8OYdts6NWOBBdNUeHYUnDhS%2BFAYfOuVfj1wQNy2iLAdRIMoLrD0s9OJ00J9NXWj6jMvHTN13vvB8Tp17oVD4Pq7aw6HEoJ4Wt5PiIHBz8Tz%2B9qfA0bviwJt1rPmZx6o5yZx3ghDBc6ylRPcHEl87uJa0zjKS98kYr38iI8PY5TIXXXVldDgTnIxmTqZu7s%2BBnE0aibpVo4KkUjvbYU%2B1KjW2UfP9rT5F5vOpsZGfPg572rhjNfkr2kE3FzEM02QAKYxttD472gk6%2B3Sar3oankuhYJdXGji7gphcCdl0%2BfKGCkHIGZzfCyWM33HCrgwrXtgHivlM9ZbfmxFwgci6llZMIGkfXPcenYZHnRzF6%2BhHtnIM%2FIyPOdfzE02tIwheGGxwY6pgEPaWsT9v7tVYgouS7KoiPrgcx2oihcgqr0BDPaHaAYx0XZWjk3TvAbPFQ%2FGeMpJRiys5qVJTwspk325oHUsWXLT529koKxHawjK8jBVksRDSJa6y77vLkYWsLTqeR%2B4aGxufmdxsCB%2BAl2bSbmCwq0YUohZEocMohg5kxYgIJEyZD%2F1YR6Qy%2FNk43xMx9W2JcH3cpRhSr%2BNbhFVCOr3h5eyynr5dHK&X-Amz-Signature=11ebf39a9affcce27a49fc5d0c4ca8d1162354bef265455e454ec6417893092d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

