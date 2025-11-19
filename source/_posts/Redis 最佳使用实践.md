---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYSBUIHK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCmqfs30L%2B1OzyNXN7a5UfSvBQlh81u8%2FrPoOe3CSx2swIgDWaUcsH8cQUCA5wntR%2BNaa6aWY4W75DdOOFfNqppyWYqiAQI1f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIeN2naGipNFPWfw1SrcA4jH3r3GEdxGm7z7wi5QeoyKVGVfWjIZv0KpEJCfJw7IfDvuUoyah5dwrS%2BjU1gfCWXjJd89Nk30vSsNcl%2FdRKIGajDRU8GFnB09A2mK4xRK9dEzGq8Skgt42d3nP7pkHE%2Fedx0tC6AcGnjiu2NpoQMKGnulyTp5BzAAkE%2Fj84HYUg9WapY0KVrSeJCVUYQAECzSomAGOP%2BXpAJPxLmPMBCNF8jDTZLdeKiDIwihfNkT35iMbm%2FGRieeSZBJa3S7HENAkXSQyisEcbHPTxZyvaO8v3FrvGiAaYsdPe2picOmrGEjV72rjlC2hndLN09WnyU8gXYiulA1Pyden7KjzYW1dAc9VzsrsY3M9vDk8%2B3v%2BFZARQ4cEKF2rFx3AieDIKHb6ejO2RgwhW4Rv3Eb4IzhzQp1aDxp%2B4b5mrw%2BlO2NxVsGbIJE5HGtWsMhuHvobibKW9%2BNjFG9op2qC%2BOOmVkRI96q7R%2Fpn82sFH%2BS3IaMU%2F5V%2Fugs4%2F0tb8uIQZmCpWTBArg4w8j6GgT2%2FjHddVYMpOADwdRiUg59R42Qc51yw8F5gNA5SGGwlfUmzdAuKeWlbGOudOqbRqsIpP4vlo4rxakMyEd0IA3un4BIVuw6J1kxavCBzCGkBpj9MM%2F29MgGOqUB0EAzaeSomyZWq1USelx494I22PxCsuXr5U%2BwAScahfuakWRp7ilncyt1GfYOPmC7kY9jhKUDUgrqugU3%2BNeIwPx1av5sfPz%2BsBGZIblp4k9Y9zzpaaMaldsDpCinIwgreNU6w8M%2BRMEyrovgj6cs5xmTSwSAi%2FPlPEtU6IsHyUC0qgueWtp8mEWUxkuhSmFIod1BftG1UsoHn9gh0X7Btzhh5noY&X-Amz-Signature=4e7a533881d6f6dc83beb4b6618e8bdd9feb84610777517a8e2d5bcfe5b28afe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

