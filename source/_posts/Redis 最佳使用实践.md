---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PJG54GD%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCICRhW0pUMjQNS4Klkm7PxFfNE1SKG60yW1upk9fuUgL%2BAiEA3AD4oPc%2FpdhOfWrHA5xgO1SA%2BDfOQ5KMGIuhAGN%2B7VAqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEj00aWn70Q2fESv0SrcA94A4nyFub0EQSXl7Genarrhctr%2BThUCiecTg5ztznfPR6a5pqPB67dIEcd7ncoPRYCyOBJV2YNyC1J5CVUSPd1If2B4XqN%2FCoaRLDTe%2BZUMV7%2B9SRB6V10Y0ZDr7aRP1GYEdLCOjg2iFlVUYrTWM4LbeWqJTb6KeaYVTOzfQ8h8yXBgkt1y%2Ba8OZ02xCSyeSyfuWuFUP%2FbvT7TFJnN%2BrD2gyRg7LcwPgjlV7RzFeODsFcyo%2FGOMUBeOCR09hM8ReM55hKTThfGYVApjJLAB55sCbjrYi2M1o7AOOlpcykmmIuM3%2FxdAFWAZYgjESUaB4ErnQ%2FZHn7wYfaH6ZMSCH5RHtFQLU6eL1JWC1lJXJdy6Uo8u%2BmXV7W0e%2FCTnKxXdEiHzq6kMijS5GRVMS%2B2E61StuGnq7Seyum4DZqI0L0NwkuvSzUWivq2G5h2H1TgJTtdNTClr%2BYkf0IDPRhY6spi4FM06sU3D2u%2FqaiH6jc7bMHbGzkSRuY3ryeO45GWOz7qrBVgcHpVATDOWe3TI%2F6NHeoqSQ6HAUfmTgRFeK5L8H5oEg2FDKe%2FBxg1paRZCK0INXVns5ZXFtr1tZUtmndKMhLgRacH5lR3d%2FLTSrTIy2YLHtjZdqpz7tJmpMNfK2sYGOqUBNK38f7mSPkBCmT%2FIlm41bFxBUeyygy7jd8WtLBpEaNvZz2Ojb52SFypPhdGh4w9qBJVESk8eN3l4sTqkoPLPu4lc7vVznuqJXQTRZUhnGMUB%2FrAKU0EQYL3hzo%2B7FwHuvBpz8BYAFkkZWPpF%2BTSQD%2FCWi%2Fz4%2BFJ%2B9wc88V%2BvXIdXJgrfRyNJoRcPuhJ5ULMmm6CnzhxYzWbU%2BVBN7Ug7wpNkzsNo&X-Amz-Signature=a13a0297b8288f95575a3e7530cc3eb17ae38bf9af253eec303294bfa7d8489c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

