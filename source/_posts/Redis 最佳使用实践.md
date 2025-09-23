---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHG45VRB%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFjerAtq2f%2Bw95MzS4BHqIzXvVB8tVvGWTNZAzG8t7FHAiEAoT9PdTI2CEhPGb0GuyNwEyj1ZearXsgAgKBTrn5RtQsq%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDG09s2RmhCd22egJFSrcAx11d8f2fRo9l4IT4%2B6umBpHrzxiTMWIrq65cXaFnjHgo2uaN7krKq5Se2VT7UGoUpdfYsm6DBkO%2FxaMVBVaVkmNmHRoL94dKiiPzlxX8ijiWkq1fupex2l4dM2yNB27mG7XVCCYsSs02nSoFErZNekH1O1Ei74y8bdd3bvIUvMWPEgZqZ0VmvOfjhc9nWhRMEI5NmqJG8kbeXa6QiowjjCpRxBvOTKC7MVSg16EnADmyezJ4tPfe%2FkO1rsqYIUnwWL88kGJVgjpyIkHumje0z%2Fv83aNZvTDGjELhS7efzPUUKsaJ5%2B4d2f3F%2F9BAwNEX9Yqkvn6ehiAaqCo9RkKZ8kijD1A%2BnZKtrgLRd26COdbgUiVIoZL5gOxzF2BpjO4jGnu1WZxxLURJNFpRxPMPZdifr6rMQTvQEndModOy%2BK%2FU0PasMuQxYjF1%2B56R3duvRNp0uOM8v3vZM9yYMVoJFlSrnjEmfG4Ewlfz4EXFyCacN0GRspA1F0QaarEG3lC14H12Z9XTdly7oaeE%2F%2FnKmiTjtGO6XYirZLOzp1zicPBzufwD13HymDQ3IDS%2FBnw%2BvGgKF%2FSwk3wfXOXDkdF%2BzNZRB5NgZaOLa6dTu7qb4fu0FuF4aVSY%2BA9ZBMqMP3aycYGOqUBpTQZZpkkuISABXhQcaYzBXfcBtoRjYk4j0I0wW4YFAYlrKn8y9Ay2%2FGIDuv5Wbe8VET1%2BKPRatVXyweev%2FpNl3HnkJsuTFK9t186SSWVH3GxvG%2ByKblyD2LP5tWszb5SpHDBItXmY%2Bh%2BolnKoaDUA4V245KCyPvCACOTShgz08mTxQ1wf2sXtLUt3Io%2FFOiLBghCEJkI61QkQ8ebblKskzdNH1lP&X-Amz-Signature=569fabe1c7f9eab9e2e9e2fb82e8154dff26dd40b0b87fb75c605c7d1b8d68fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

