---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWCIA2EP%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIDGWL5aTONU91RJHWlwBdUraf1JKY20HhTDIyQpgwwVfAiEAvgjPkm4T2OBiWNWqDYJWusgYNu2B5zZbMMUbWwykC60qiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOugUfp90UJENbjLCrcA7SsIQlpJDsUuL68zqOzBbC1LVZo4j%2BG4s437PAJesqc1sz1rfq5ed9SipyIvmh6U13uBeR4tgwukDmWtMyjSWfafujp%2BKw9MBsXiba6%2BbSXEOIPrr%2BJejAUetvOjgN%2Bu8ij0EctzRd%2FQEgkDu2L%2BnConr%2Bj8haKdcImJBgY4k6u6q5%2BBFNjl9ebRhupkFyB00w2IpC%2BH%2BrjETuWcE2SjRcllWqXmMTMWIJNhBYcFXsf34Yk6dLrRY73nKKd8Tyc1p%2B5VtUvhYCektxO0aVgWH2akTb%2FaftCP1EywabIB6y6y7E%2FHBzqcvU%2BOuaf0S%2BBXzuzmK49IitCJ48DJIM%2BTAR1vNkGc7GfrviDULsTOhFiIN2kaZEAJD%2BODejljZBb9h2MglDY%2FYNvfEdY5gLryjrIgUThqqbUSlL0juHDBUajbjbt5%2FTmOU3oA2CB4PfYz2SwQV1TNTuZbx2vJFByFZJn%2FTig1LI%2BhEw8FoM%2FdljgDDmAmbWuVpUtmuCJodY5fArMvKD32%2FpG2SxtGwkbdEYSQX1ZgW5ZBRi%2FTmOPlzvmduE4ndjn8Gh4JTDo9lRKRG%2FdHOf%2B8Pd%2BS4S8epvE%2BoJF29U%2BKLS21rO6n%2FwQh%2B73utXsvUUldX8QeOflMKbfr8YGOqUB99av0G4rfhPh6bAjCoPCwKSIcZuk7TODu%2F8wvxYdArrdeT6DFLhVcoe9e7WsXsSEkPIQfMzwIb%2FwiWLI71iF95cHonbvI%2FWpe5y9JkJEqYZsWPEP6MZ5SJs3RFQL0PCT%2BpjAR7gd%2FTZVmHnarY5TZJzl3vlOQyRWj8zMjUvXOuawh8uC75jgbwLCYvzxbu93z1eBjYtOSKZDQ4zJ1ZObVP96O%2FLY&X-Amz-Signature=0c020c4da424c725f7cdd288d523a26010203722dca5cbc7d32e0f07e17d434b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

