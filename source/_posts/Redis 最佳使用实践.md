---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WXVBRIP6%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAeEVsrkhtWlGT3hr%2B4%2Bk%2F5gMir4Hc7gtqLBUiS7xNRjAiAYJQ9MzQE3uGQDDljcz%2BUDi1pY8ppjuQeJzOiD5jod%2BiqIBAiF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMp962XUSGUoNzTb76KtwDQxl24mFxw%2BTyNjbv4sXoc8aVjqupXzD2CVIXSbDssbjjk6W3Gt9Y1InTwkwfdWPkKfNmAA4xM0aF8WSzQF4zUow90nlaSNUT54EUvz5I1uHVPpitAS4Hgz6i2FdOq62E%2Fo5rLZDBUAwJzzQ3MXqyYwL%2FFpmznXuropAnoN5X2pzwoUdZMoIC3h4UOl9Utbd%2Ffb4Tv3YYYMmyjHW0FbPR28tcCqMlwwZvWJ9JyTfyHed8vELiT2tPqv8V9gSJHBTLyOz9VZDJi%2F%2BTNIDEDCPsVPOgx2ItpLm%2BTBctB3JrzsQdbMJNBvAub5zMOzlY4v7zfw9pdUjXiSXRZvCI2bz0QIsqk6ULL1UBNRom33r%2Fx1DJAO20UXBjagBQsAhH8%2B%2F3slu4leVjCTO%2FvKqdMKXqVs6M4%2FHFgcWrFuTXdfs3zu5%2BtuCNS8id%2BdtGtLjM%2FUOAs9yRRYk1T4qHSxCKysFzGTubxGcxUlf34Pby5jbwYQ%2B2r%2B%2Ba6IYITMSZ8%2B3%2BeSfJEZF4dVwmrtxgWqUuMrapKLnQa5p2DoA8hgORChfAIMGONNhRdfFz8Q1%2FAaPtnTowIqHXpK273WCufue7qNmNogJZD0t1Wak68ZoOr9LeaJmSOu1cn7WgWpIEY5Ywv7zjyAY6pgG%2F2nmd1qJyskwX2TBAGgO6%2BIbEMkeSXXEjX0Z9dVmTdEtWWzgCFXfha7hBEDA0qZMatIQ7LE%2Fv1nFbUfcQ55hKgSSbYGDrkuNCgsNjbOeQPGkomhzVbwxKET7y6hoCfgzaTAZ4h3chIFhW9BQlIQe6UZL6j7lcnmUksA%2B2pAv0xGFkVVT6ylT5%2BTcl3NBFOJ3mfgO83l8l2KatVC8p%2BRRmPP36K0D5&X-Amz-Signature=1851582a8fcb74398c9dc7739175c899393b4052fdabe7a5e5db792dccc61247&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

