---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ROT5GX3%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIEWpEwQJSE1CbVKF1fXxIh39ikIjd107yk28g3K2hIY9AiEAhFQWRHpXPHKKv4EbIs4D9W8mjOGd57gKwh9wL16FThgqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCZtEdXC6wHHgHZLPircA6DOvhZSJaaV1EyEgNvlX7JY43%2FZ9KjXjaCiHVOxUa9gThEYxDjsmzvOp6C0%2Bc3ZMGlBr9U4ynI1GHPToGSBpoN3R2PBm7qL%2FMIBkBk9FjzUnoESn%2BalbP8ublGIXNhAtB3pC8qvOdvxkRYzAxzR7nl%2BknGjvN4CeTWT58n4xWAOFsMgzIsPkvOUNzLxZFEqgECrI9ncyN%2BrzsAWkdgJH8jhqIKQuM3i9O8cF%2FsileTc7yOWlX6H41BqCROUhVB0tC0psDAJY6YMmu%2Bekcoq1I%2BCGqWPHKtm2Iyl9btkr6mhZb0Fz32jmLz7NNym9aEqK5WzSfD8asNuHz4VehwzR3GPH6u1F2%2Ft3bh43hl3LlbWUmmDDQjTGjLD8NfNzkRkCgovWZe0mTbE4rGCphpclOX2HxaOIGuITLh%2BXHm4m%2FO0hH9fG1JGaE%2BhbzhZG68m%2Fpvbb9soKrzen1plSSFwSnAFNM%2FSXtVUiLHkIncCGK93bUVyVcqMyPuOMZ6ZJEPVjRRVpM24IxCc2B9v0fQb3BTXIpMe5WYLyNAjgwCXnGCPZQNdkJ%2BW4W6W7x3dijoLXqOtORBCiFZGL9JoXigAozaomz8WH6s0r4%2F1ylGkWvx1wNpKsfdsTuGLWi1DML7pnccGOqUBYAhkwzqYIPX1TG1pG38lYN1Z9VEACmKzLHhjWdgXuMh5MBOzdJ41%2FRTdkBiyxlgFw6iJc%2FJR7kekF24tJTk2cEskWG1zDtAd6tlyx7u3sFpVHDW7xe65ktaKBw6JKb0j2bvvBOWQB9qMZLlGehqPkZwhpCQvZV1X4Ttb2kuVt%2BrvWT%2FumMheTDxlFYZLtY6g57GYemBq%2B0LWVJcPaj7XmO9YIG1e&X-Amz-Signature=d4beb856acf25f1d4db5e1ab38e0d28546be6240d488b90f1253e3cf1cea46dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

