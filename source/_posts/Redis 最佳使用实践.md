---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V46OXVUA%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQC9RjdydfdzBdIu%2B%2FnK4CRmoC6SgbvVadDfOywE%2Bx00YAIgY%2FB%2FuKOol8h3RdOJdo6NgTRNdV5Ztt4y7d4awnbv5Acq%2FwMIChAAGgw2Mzc0MjMxODM4MDUiDNRFDuCaOQcE2OBBgSrcA1JW5wk3e3CFbnhP31ppD96v0yPyf47zZX%2Bg8rKjAJpqWBpHakZO%2BECbarA%2FjYbvIKBUrrF8Eii8gk6%2FbiyWkP%2FR8lXLK%2BPPgMMv%2FO4wM6mA40%2Bz1LfZE5NaFsR6pxH2%2Fvu1aJZ0hCHZiiPkxbz%2FLYLCdyFDRh2AqBejiLU0qmk0T5WNOFqWx8vAptFgYVfEJ5LpL9sohS1q4UfKskrDz7O4%2FKGEDlqyObh5AWf7D%2FqMTNl7Tsb6gsREI4DcsthWMvTaSb5A4rCbqoVOeVxgLsJ4MB39PyMLaWgSig9qBN4wOGBBUlRBP8Re1I807C9Vrbt%2FFdwtiitYc6spf%2BFJwI0f4LVu7MeJGmkUd0zpWuJr93wJ6JKlKIJ89VrsSKUccNyfhlGIBoA4qww%2F8YVyAhW2E3GHpzi7I08lRhpQ6JnxKqbqpBLVphMu2I3%2B%2BvyheCH5y%2F0gYYB%2FC2BVVMlqLquACCBNw6H7BkR3H2Uns%2F%2FsP0g6oGIVrNP2ujXSelacvT37i7j3KorTYoiCeZAzdBYG%2BGzWYLQeB58hdMijUUD30n0H4%2FKOVQGv4v7YTGbQ9m9GRCtYizbBrA6ku3cBCEjSuiomfQfq0dcePyFaWWY%2BQ7GdWAb4Epx%2FlJOTMK%2BryMgGOqUBOtSocY5tqQEw5u84JOJhRtQhsuB5Dmq5w%2B6MP%2BmF6HbvMw%2FQ%2BgJSqosaN6p5IF45fPPQMivpvv5YEuQjbhkxEC6uOfuBtg6ee68Yo%2Bd9EYM9hRK83noNn56kqTzfxJfCOWCByRy%2FuBWBTH%2BvHIV3sjdudv9Vmd5WiKi0NqtK%2FMOwEkS3RyHLF7g%2BgE6fIS3fSOOwPffnD%2BbvznjABgJHiQEwLFxW&X-Amz-Signature=cdda5d5acc38ee8c6c2ec7092f248fab1e55306feb35b41e6850fd43655f9668&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

