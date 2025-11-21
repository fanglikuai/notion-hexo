---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFAZZ5TT%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIHfNnwsVvnLglcXBC9ZOW7Q3aRlteTTAQubWrci9i%2FN4AiEAj38M%2BQaYQRLVywLKZzxmchkQkIAXui3mv%2FSGc7tp478q%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDIDPzLXd%2BXAiXumi9ircAwSmOvYFryQKAd0vmLA47s0kbi47KlnR6glf9zf6WulD0%2BqptMliGyFNMrjDfha%2BpQr1eHTQx5Zf1oot9g1LSenYWmUcXdo54PtUMKL38rbzQq1qgnq0SCrfUanzZ5RYXUn0OoXDIbZOHX2GV8%2FxRrAOtJT0bube4QdhNQXuZafVsrN95x98SSPGqH2gPVGkz6fi0%2BYgBEs7FeA%2BmBdW3lL7fJWb1gzJcxpu8Jpbw6SpJoQl2lcKYQZf2KxJdnaFElbT2BHUl4s50oOBDMCWgs5I4xHR5BAThcMFr63%2FCafn2YGzsuY89Up4nPGQEDL2oHrMv8iLHgfdBPQSe%2FinRuUL34RDnzD36g%2BC6ZddT0YCTnOx4EXZqmK7SBoWA%2BDE6FrgeNSFOHOkVQCB5jE4Uov03ztv7081WwPDa%2FC1zEjtEu4faNuDM2b3%2BEFtpWHc3twKpoWbzu%2FjVNrHZBwvmqSYDGZRBpoyq7TKi9jNNjXcZYMXkLwB0jqETWY9s93RKuUEXe%2BSHHZrhKMtKaMAnk96%2Bfk8TrcgVMUwwBKFscXRxKi2cVH87qdOt%2BHC7IwbEJObERyHCXGjEhZ4ZMKfzfFk23g4kMYbPHWC74z0cjktorSKH5Zv07ZO83mtMMqq%2F8gGOqUBoWVdYHM3povh3j9sJ77m8%2Bt%2Bb6X6UATD2M%2B5F0UJqfmsjWHqWCuj%2FXcID4fDvIfSkR9YW49Cwbk66sKBnLAcSr296fHbRrPskBMYt2Rfbo4PqV090S6nMQZxF74EgQRVs6ECocRfTwn93Wlt0ck%2BELDqSF12fWTmSjQTuwrk1JrDpi4iXVRVAL5zx62APD5yDBrpvZD5gmhFSKDU%2FH358A06Uy1M&X-Amz-Signature=04cbbec26b34d010847a5ecc9ef18d233d9c351fb877604fff290722a58eb9c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

