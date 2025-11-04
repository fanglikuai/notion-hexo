---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LYFUVXU%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDAMvfnkRrk7AZw%2BMU5yBI44rO%2Fvzj24eYHj4xQql9g3AiBfer15G5Fv5R8NGiubr0fdxYaSOV2oH9EtHtMd2R%2F8LCr%2FAwh%2FEAAaDDYzNzQyMzE4MzgwNSIMV6EVZ%2FQGnEguXt9%2BKtwD5H4jrL9nUaaeUfe8gzeLHqDxfu7Katqu042Bh%2BQo6mHYVImmI31w1KZQjke0lPvAyPFNZciDgvLfg3jV1k4gUtUEtTdMWzalwaehLwLfj%2BJqgD%2Fny5SniBDjRkdwxBuyrNVTpiyPuovgoE36%2FTH6KyVbMZOhbMhYosl2bLziUjBnYZR1P%2BuS6t1KWB%2FBqmWS%2FM1hO%2BB2tSU1Lb4HnO5Ol5sPqHrlqJ4byzJnzR%2F0hotVuDJWCCPW4YU%2FJggguVYbc%2B0O%2FOV6LRVZYzfVxDqD7LutWtxKMP4VQ0qXDMRJkRw1Xad6r08ot5DkH4smrla%2BF86S7B42a%2FwvzK7eWm52vSuVbdftKU0GRPJhvQvsC0bEUdMUTRVJBp7s1%2F%2BAnOlLANYZ9Nos14JhJ7H2%2FhCYJ8ZnEIzkqXJd%2BqZzNm%2B7HUfJ5IkYz%2Ba5%2FI8K5gF4enPuS9r%2FgQ8eWwit2zmSt%2FZAP51vbqEf7NsHTlLTwWTZnvZj%2Bak6HF%2BY%2Bn7pVPoF98xq9M8lNHLTwt9Ygq7ftRyqT%2FHQTbMwy6kEImHpPWQwt5GFL0wMMDGZAeyiEsz0uYwio1kgz434Ig1gHm%2F6uwJWlJZ2jDfVFQEBsF22l9xYUmC9pE2r39oRdfWKGQow6uapyAY6pgFcDmY4OuIoDkxKKlUce9mFSVwjgcxdB%2BFozPtHhuuOPssYPv9vV9IdQuZzJNx11KCqjCs2P30POEawcolQe3o5nQEJbcolvhIaBxTt8oFmk3P0gk4Umi51HD41SRHc3uiFwfJ3hnm3JWdktTk2%2FyweoTmoHTvU%2F8CmMWP2eL4oKBtDWNAq68liD%2Fn2VGgDQHNgGKN0CrbFuRvOynldRg0yWfsdkxLH&X-Amz-Signature=3b31f45cd7c02e0ccc9186f24c91978c3b333e773d7b1968aac0181b90b528e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

