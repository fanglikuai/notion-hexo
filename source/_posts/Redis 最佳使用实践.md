---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTRZDVZ5%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T090043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBTL7FBuLDf3bcGMKvyGM2otF1UeVGR%2BqWypHFfBPfBbAiEAnTDD%2FfVjfGh2odY4MZzr%2FWQ5VVXb1Fe1IDp3V8fdTcwqiAQIov%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOAQReil%2B%2FMkKJtKJyrcA3WjuSVYV5LZdnp9GngiNfV3fxlq6A%2FGfNBRtpzBqO0%2FxO5icMmqq59qtNCZaesUFfdJJsdcbQEFoEsAKXasPWQaU50tK9SBSCfoqZupjaoZLukUK8vwGwX%2BiAXKlbl39b8bJ8mY6VGay%2BeUCO979X7gb5vKbTV9bO6b1lmWxss9P%2BFyojUPsqTiCFJs5RNsQ5eDMRJcJfM2tlljshls92vpBGXhjOyVAf69mD2NtSTGj70IziSSZdagQtAGOLEYcOPgFKYvb3kl7yLqo8FUse2%2Bu8itGdQszv0hIAHERx3w9L4TtcCnSu2DMBCcKCrlFFh9KhsPu%2FzOW4pD3%2F%2F5haYJdRo2vUakaaSH0bbEn1YoomOWMmvPkGZDheileZlls%2FMluTbxkPC%2FCKMi7DUXlOi%2FbEY8TVCouKC8UXJduoeULXrjFIZzSnz%2FNRL79VHMzN8nhxtRxe1C2HmNLME%2FVpYUj%2B0eS1f9z8qd3Ncr9t1rMPgJhwLbq7SLYWGbHMX4Bxx7yF6X8U00ejK%2FGjtp%2FJKFEMZsKepQM1Dz6zbGK3mHYd1RI7UK8QhQPTAVZ386o%2BNiC2PwQyQni%2B9X%2F6oEZ7cAK599aPIKWpfEO2I5oRGCqz3yIbRvTHHZbt8aMJLDscgGOqUBQce9vcsu7V8qBCa8J5s0yDd7tdBsK8HKvWaZwJBEpaMT2H1176XpER5DA4%2BqJpsxeuSkv9%2FQ7wcK%2BOD3t1PU7omp1eLQbIGmUfdz2FrEdjLUyDlUIM65hVf3dZxpx%2FBGer5tpx8lWHUICCpCxpDj0xGZKW%2FbV2pj2nD5rgHM2tQOcXZxtw9ZxzYAxc5CsA5L1nlePjQg6QIZNKyBriVOzxfea3Uj&X-Amz-Signature=8f7919a304b53ce8581ca12cc206a529e30b2e451dea79f33904a269897632a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

