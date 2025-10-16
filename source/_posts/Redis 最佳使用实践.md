---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCLQWTCJ%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhvY3N2%2FIOymW2S3n9YfGslnRWjq2evBmu7oVBkcFRGAiBTt5iQuT7FLYTnJ2hq2dzXhJjWsuiwSt1dM72i0BjhtiqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMftbvw96Efqz%2B03A%2BKtwDC6VrU%2F%2BDseMEYlhkulp6BQHt9ScOF7fU4Qogpb7P%2Bu8DbdmxL%2FxtNZXvPiiZ%2FvBfXdAEfTIBgajAxF7DN9Awf%2FVZ1wTdaWuwhy%2FMuxjwD79qGDiWHzATcGZ7AxUy%2FR86%2BvJjek%2Bta4mtDsmD5w5CX4msRnBDNUW0N1d9al4mFhyJVs7a1PTp82E2yPJtkRzE1HnkDcWDovNNihQFqr4bH9cgmOyE401DiVoYG1%2BBOu8c3rMzk56qNlbOyXJYz0L0hXW9S2n0NSjUOvlK0KSim7H3%2BH0KNG1nUW5E273JJ2txUTjpStmRFRpwOhw1CGUgI5hkcOfMmkZwUh4PTjWtVrwQnkcerbAMiZXCt8OJUb%2FcTgWhz1hMyWPvJZA0Lda97VlrYgFkOm4jYL2baW44mZvQiX6PzF%2FWVno8EFZY53ErTBWSFNKO%2BWLemle6%2FT6bWKenQ%2FwT%2FJExUqBT%2B7Qrc1lan0PG19D3DmgG88Gvje0tWZ9auQfyPnfpuyYQVmFtyNZsC6CZq05sOO2ZkMmPd8xXhpYAgh4dve2aymVhlAtiyoQwyrAijqUk%2BYF0US7UiB0h%2F4t902rUQuPASbYaTem2Rv7WgvnAT6zmNnXYx0RUnIlNhR1BPaDoxtgwnpjFxwY6pgEUeKiSRRND3%2Bitvgl%2B26PyrhyA42d0JlgHVkm5WOSuzwtc1anHqIq08FkETAwnBQUE%2BtqtYlKk27SJ9%2BRN3NlwApPRnHoPgeNj4Oy4A9dsys3rDAGA1v1ooTco8sOxfV0n6okTbwvLEHoKolilpCapfk2rkepNUqOP9YpVdrZlBVSQ7b1Kut2Vc8DQHVmlovqRw%2BWDH3Z1PQhauymZo6C%2FM%2B0sQ1qj&X-Amz-Signature=a96b645d2e76f61b5d0bde5390a29b73ef3ddb7619c5f0da6a41aea265a984ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

