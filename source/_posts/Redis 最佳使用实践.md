---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3ZTOAG2%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIHC2y1X11gp46P0%2BiS0mxgqHLthyi5mjMq%2FpyEqjxW6wAiEAolhALmpbJEoC8R8nXUTa7wD7HaW6MBwuwidHjEcItZ4q%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDFxPqo7zYUliNUcSgyrcA0ae0VCbNuSABK4J%2FbPXY0anGmWKS%2FogGoJeahoFKP6mjM%2Fr46mdsQJD7gd3kah4UwyhGasoY33dQOpkYqLzttnBv1dYTolHuvhk%2Bnj1IbZXkwOrgSwAtS1ixnHKlDv1FgJWu%2FV%2B%2FS95y%2FvWI5wz8EpAF6SkikDWZxV7GvX5beDP7Mt4fsbCspGK5dGBf%2FoPTip0UzGzkv0vmlFQtrD6%2BOZeCIKU3jUqnGswPNoP57Vxx1e00D%2BF9CVSEZwBM7CPCPMIW5uVq80i1KCrSBQTVZfmxdJcRjinM9AGOXIKJxtZWLRK7Diq71oXfx9aVdtPcazE0RrYSi6rBKIrk37wovw7%2FtQ%2FtDUKuvERHSnhAsXg2TGJKmBud37gU%2FMpx%2B2apVMHW4MvjNwhvQC%2Ftg95zXuxCVLqUIXrjNLcPf7W5fDsSW7ocjMAdBWpg%2BaA5uZyrXzOrhG9t0gz%2B%2F6DsTt16QS0oMwjlykXGEbaO1epW0WAbrZgC%2FKRPiqCqKTGpJa%2FBMjWg0YqZQ9jpuR83ewTVYNxI%2BBdIklQph9tCvHfhK%2Bp%2FCqAZ8wpDxwOm5TdaBtwb2GAc5EYdRscwvA%2FCPVplDkPuQ0rLvNvMA9KrZjKKLyJW7ODvvz2afhpdoMvMKCJjskGOqUBbd9pBzkYoaXWnk%2BRZyI3Di%2FctG9kT3T5%2BAB%2FgffSSx9QNlNum36CbKDW4GPwaARIwNFc9CdWb6XgASljo4bUoaKdYF2EuTl4UwfiZUL2BRRuh7DtZBaiO6xKT63wSWwjPa23DWIfmRIuxD6oK1WqhbZZiHCxgnYZaAhW8nGRSq9rWTDUQBYb2sxZ%2FPT%2FpUgbQRQN%2BABVgqIcED4Q8DaLSMaFgIU1&X-Amz-Signature=6171fa48d39cd44123b1caccc8738568b8d64dd45f3b27e1c30bb168e7a16c2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

