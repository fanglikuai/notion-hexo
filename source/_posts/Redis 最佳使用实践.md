---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JHTIPWB%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJGMEQCIGNmbiQLzYW2PoidPiJppT6MS3s3ImAPD9OTyIxo102iAiAfV7KoHBQnfuCdoRcpFyfTzIQU78KK4BlljfL5OiW68Cr%2FAwgxEAAaDDYzNzQyMzE4MzgwNSIML%2B4Drda%2FDrUOb%2BYYKtwDmirQ%2FewNzCoq29VKUZV9IYty2K5FqG6IMr%2Fz7biksCEAw2mLGgbRZzVMyMGTZUeM1Q7dzrr78JuRV1WraZLwKf%2BKU6G8qjFLIURh0iutLgo2ErGJSmEtDryn2Tim6t5xtkGEO8jiiCNhu4i6ga%2FXSO71pZ8ltZUAQbrj0kmlE6majUiCJitx%2Bo5H13TKnnFSeiUev6MwpeuGs%2FrETtKilwPZbw81oUOHipGSnyEViyINc1i2FsEwyTufMIiWzWWnQbo%2FrLRq9K4ayfFLbY9PbzozV5tIgHpNcFWlp%2BOoBvYzZJhyg5pq4iIuR%2FnD1Hulfg1giq8Z3JV0nqDU7qX9bUFyZgTIL6lVH4AqlXAkRdJZqBEze3ISI%2FZr0HKIqi1J2zqbnGZ74TdHNHuN8HLiQJc5MRu4v3Lo%2BQeFZ2lIx2XRUNGm%2Bn2yqhN5%2FBnhFqreq6djcSkB9XdVzIixm5PXi1NOSFuJy5AWHk5JUrcHBc1M5%2BmmXjhxtKp0nvRhpc67uwRLKFeeNEF%2BBbvUvcPMUGDZOerKn1RHZ%2BFmQllJ8gZOncn0bAIt%2BGJYp5fjSnksFxJpZmiQvZBtAkfyugn%2FRNKg5c2P96jRZiGlQSH4aMdz3VDoPRMh3AKz%2FqYw8fTjxwY6pgHb6%2Fmq2MZbQ9U14Lvu1fXSb5KkbyBmScxjbD%2BuHyVTh8XsLrstMzurzbrWKOQYPanVy1z7Y%2BhALZcpAkSWLESuBQNA0VK%2FuHmi%2BOWt7nvdsXQx1Q0F0GUBS9URwArCHLhb7AM%2F7a%2BkT8KG5Jda%2BH3LZXKczLVymSHm552gi0iZfKwGqhGqedxb4XE%2B%2BVD587AYHnhPkevqUCPeF%2Fo3ahW0n%2BdL2gJq&X-Amz-Signature=4a2405a81ccebb65706a6f6d81d6112bb2835bad443950a5a843f47836b0ffe9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

