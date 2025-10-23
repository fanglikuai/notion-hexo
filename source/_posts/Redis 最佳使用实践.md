---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LCDEETV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBv6w6n%2BR%2B8RImPW6K6KCtf9Mg%2BnzNOTt%2Fvw6r2gJe4%2FAiAPX32FADjYBx2Q76QGfpe43zlpvPqMAMC3gr%2FC3akqjyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIM7kHYbxCqpDxXThU2KtwDHv6ueFlc8m57AdAtA%2FNoWj%2FWOpy%2FRLPTkr%2BwIZEonqHXOhAlWr4Yq4%2FeN5LcKermvAWYhMibV4%2BtzY0nV5%2FlSuM5OyfUxmD29mIPO5p2Jri6wSRK9HyN7kpzyQZConkkk8BhqFL6DtLkFA3%2BRvOEBx%2Bng8yMhtDLBN%2F0VM2fiJJUiIKJ7ewwWtm4agWd81RDjjTCw3XyYNPN0G4q2xXzK%2B87KECd7dauX9O5Mf2FgPbk0eYGzXN61EmSZLmDCrZDHNzJrOH%2F0yqOblpYzGwRjEpzDkUlRXt3u5W2h6NW%2BUn00jjLmsxOXpKlC%2BR0hGzVlH7FswJGdlQePLM6MUtwtvMXh6pi0U%2FetIMVo0MhtrJaUb9SoEs2mUTir39a7VogcxTm%2FoigNmfoebH1dQ4ddCEzwY7QNoeOSjlz1v3TTmO0Gc7CTnCbCls87wQtVDmOhF6GqJ27ifj40w0JeevW0eUdGYGzIlhRWQcf%2Ff8k5HcZEjpCjd5TgewlJsBGUM6jGzIn9PZ8FZiwD51qtrGEx1Al5oedL%2BN5z6xiM4jbgD3Ap9VdiEwIlnGv9AJFUYHq8bbNrtKKrAoOFOagaUB69iaeuOltzgsYiSkltAtQlCXS%2Bj0w7hLCGn4A%2Bt0w4pzpxwY6pgHqT95uxPdlKvQV%2BX8xYea7g%2BLIkwViSTyyNeLuyrYx3LBbg9iut1VTGnM6p4HIsupuVq1pvGQmQUJoSbG9a%2F%2FVgi6IKrcaT2SsmT941BSt8FdqgZpS6K1F0w24qFhr7xJQ12yXm%2FV1MxafBZhARIunFdtBV5%2BOP036Sbts5Y5xmCrggBKFvHtLPPZscVcy4O98gZTJXYVjil%2B36iAveYZieNmha%2FFH&X-Amz-Signature=476c17deecfe633bb223399207ea0364b45c7866d46a508d6f713dbea15a032d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

