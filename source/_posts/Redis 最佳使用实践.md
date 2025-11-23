---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PHBQRXE%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQD8iRHbqgVnAIgHEuN%2F78dkxBl5ncxk3LqwwaRpReciRwIgBfRfyikeBhrc%2FqaIWnOsvcfgtO3HsJISUB7VXmf7MYUq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDHyq3g01uojACBILrCrcA%2BiQszjlh5DhWlGxlX9HCXAJBY%2FFXQf3X%2B41VedUIIPYuXVg9mf4hpTmDallJEqxE6mgq5SVk1iz88iTHtZLcHLwOi6kj%2Fh4qtCC2JqDF23hXMicf5yRYD%2Fps9bR0q9uITvz9lvBNxtmUXnA3Q%2BgeM9mE99yxuvZ9b2SlVIea%2BA8HJSRYl38eiz1IwS1%2Fy%2FpqG%2BFQMe7pG0grpHtajH%2FFOS%2B4fmzVrBnC9jp2AohcZh%2FDYP8Iv%2BmdvQj17pUv1B5l96L49RES1vds2ZILt%2FmS69XNtvFjZoTgjcNxgWbbubJZ9r%2BDhimgpkt3Q6F8mSeJyA1Nk8Dy1at6L2pRG6HSnWgD%2BdtlUC6e69kjs5%2Fn6AEJDJoCpnJ81OOJHOySPHKkOVPfDaV28cDWDKFj7pujbFOg2o1UWMTJzBatOp97%2Bectgboo7NGaqEA0iAyK5dGtmgyOdI69BPye905jN3enToWZflIH%2BoGONKbBgEckSHzVq8NQsraEkl7osR6zv%2B%2BvpwRDKL4t98PjnB1R0fFdUoOiJNd0pmZOBXHAGeTWKQLEXxoG4C9SWWKUsfLaRLAmwZFxuHRSSn2zNWcMYk1l9IpXWjDQtLsIhYh%2BrEqDd7sZRvqT13EqGsxfnsKMMGPiskGOqUBANAAJtQp8Zn0Nbz0iuzfhVZS6WCksW1kB5%2BxK%2BihniwKo27uf8fx7xSL9zV0D10H1FLbp3UtXiosaAFtGisvq08cB2YKNtJQnMHmFB5s75WMTTKmbO8wgalP37TXSUSTPnzFrqtiDjod0%2FVDpiYBrhZ7zP8wUzsjNjPcnWqDy7Xey92XWO6K5%2FwJoWx6%2BC9TnoTLYlwHr4XpTeU2HCCwNXSzIGl7&X-Amz-Signature=7252650ca1ad90523ec023a6ac874a2901b94111028e04b1bc03974acefd418b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

