---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EIR6OLS%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T150039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSkIalFRpLYplhnrbI3Jesvoj42Bry1pyu56gIGChyWQIgNJVT9bl%2BrsJfNgNz2PeVadp0onlHvvGA5evWiqQ3w8kq%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDBts4vz24VCqCuvW7yrcA8rR4VfalpSy7lcsa%2FkTKJd4ilFzmkgXdfjoYeboPrQTDR%2FvqRAAT314dwQcBOqNpgv86%2F1xJK00Xk8LlTv7E2%2F7%2B9AFsdi%2BuK44ZIZrDWdFS2bYYveid5CLO5IJ1liGTZH6nq0lm%2Ba%2FB7JIqubyngUeTTr2ZjPMv1zs7btYeQVvkRCVr556ps1VTWZ6bUSSaVTXe9NsBpfWHO2DpUZhhlRd%2Bj%2BWly2jGl%2BOfdywtXR2FMwH3jipobZGPrZBFCcUMpOOLh9klpYXHFW2u%2BvTuL99wlpbSEB4fazC1HS3pdoxqdNfXwDHEOc1Z5Alf5pKNOFk26kgVLkLAFn8P2nfkkNpR%2B6biH7UOpO5W5nEh%2BH0QXnwRI9zRmzt%2FvzTOAUf4ffRZfUZgsJ1zWyAxTrjvOHCtJyKE53VF3aT4aBDJ1a3FNXdGR0t3hh14lVsSbEjqvn6TX57XoONTxeb5aPUNa4aLgMT%2Be%2BZQbJaxcwQrVOlQeMQ9CIbJVzbMm3i%2Bj9Iz02f1vBLnWhp0q5v4KhO2%2FZO3qCwniRpmQuIdUSDtMjCs%2FonZuC5NFsqe3kIDlk1fvBlasuEIUoM4XYrgYYKQZqzV20VB1zNl1cpOC5JbIM6Gh43el5IUn5feai%2BMM%2FfvscGOqUBdDZW5Z%2Ff63J3YdjBlJ9vD97BYJZ1Dg9mtCO%2FhrcjsSuzvatKPSy45gp86yTAuCmjrcth0RCnkx5PCSd%2BqVawP%2FXlWkU%2BBnVGbC7KVvGLrE72%2BaLOt9%2FsbJbEuKIYWs0HFwwQZLgjCv4e1gDPmcmJ2DHUxtGqu70Z9odaxi8hC0IF93sKAvbI6iClj5%2FgvrIeFCrTx3KbCQzp1bviROFnNN5dLKOO&X-Amz-Signature=618c0fbfd657d713e6fc90e700045b2303f63f6cdc8efa30391424a869eb5d76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

