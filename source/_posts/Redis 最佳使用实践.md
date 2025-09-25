---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ7OYEPR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAhNYZ9Q74PNOiIbYgXO5CJTzmNvr1LIfNCPThr1h3NJAiEAgX%2F7gNIQUKBFCYowq7fYZSHjbWXQ8qz8fHMrNNFHvekq%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDM369VTMy%2F6zA7AlICrcA4l8pChqZpJA5FaV2P9ecx4JbnWLyymaXLqe8LKWd4%2FJAXqhUdMGUPOvIijjRg%2FzCdlAkPWoz5cMpMi9KDsXnO7Ch6BGOE2Y1hXmIFaKUDrXNupueYC0wTdXnfdJWqx3ucPqIwUmZ228zHNqktPpaKK5fjxxzGnpormDaaS4UJGd%2Bllf6G0nR5xiv7ve6XA2s1qc%2B5qPZKVdduYqaE9vLOlPxymlq%2F5b%2FhuOtyupnb6CZAYkvUtU3oc0pvLSOACQ1R6MrEhvQaJa8mlzZdMBcyvYwrjyS1VsLuYCymJiky59sZiMErLkbtB%2BGLn6nkDzp9OoSuOfMqTjftYjOD9XjAjWT5zBZ0FM%2BGrMgbE6d719XDjKwzELH5xiGURqmYzl5%2BDcTWNLLD8vD%2FSSYkHhmi9nmyBxTn9zN%2BMBB6AZ3Thupb%2FU8IfwMz5RUjyjRSi6LC6hppT4vYhc%2B%2FXkP4Z%2FGJ2m8AvgGOpTmLeN8Q07CQ%2B68yoA8E7y8C8b6h%2FM1bqftj7C1Wla5%2BoNRidngFQ8yCDfCO%2F3BXaZqNlo6B5EJJggIHPCd48BEATds1HsGH4GC9%2FO4yDRlU7BwlwTC%2Bw4wjcAzXpUyLo5NCRd%2Fw1gSG5Hvz17EmPF%2BUiD%2BrbeMJu81cYGOqUBbNJ2ivZiP0605ZhM%2Byb%2FnIDYDmytH%2BNyEiVBt%2B8cW5gs6LXEog9PbiamvysGbMQFv4N%2FPplb0Ds2vTjQLacMTv%2Fs847o%2Fkr0xrZL7GrKUv2S4Ig7P3sVjHa0NIxESc86Vu7rNjSHVaLOFhA2GEMEC6DPkN2%2F19wBoMAXhD1hqZryGR2lxmRRLXyYKzXSyhbb70pfQKEvkO71HBL4%2BfQt0j%2BsYUO0&X-Amz-Signature=38066c32eedb8554bf68f82cd8c1307cc0fe46962a3cca39d973f2388bbc573e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

