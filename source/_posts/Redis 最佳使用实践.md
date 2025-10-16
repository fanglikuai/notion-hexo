---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUIFQYVB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBYKV3zvaEzQT1lHKcaHaFXafDm5vKfiAWYcBB1iJ1SeAiEAg7oiMqw0GHsYY4QVC7EPjq8UuMfIz3hCR%2Fb65pUEazEqiAQIh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvZD5oOu%2Bs1OsEU9ircAygD3%2BnEVORlh4wP4RE17evRzhCpufXJPsPDUDnscnKb1dPK97kPlR8ReM%2B0AXuVkmKo0Mgq3M%2FzihimxKNWZYJLbmQFpZo47H8tgfp8VtHGsjP8pEIZJxcaUYA9FbxRplC%2Frr9kZ%2FFAPEyBULsYsz8045f%2F3OLMXWx%2F%2BGiY2UXIqfEbmXJ3EH3zgiN8QqUY%2B1BLH%2By0CEHdKJslu0y3pschGmpO8%2BG2woHvDt2KJLFVvs8EmGrKNVUQRwMRMMtheLSZBhvB%2FS1oy1J5kk%2BGOpES6ZGIMXYp1zcNQYZBDTQEilJAEHzKMCCSfrZrqPS0qBwLZcxf2bnbD8cZOqyG9h1qC%2BeFc6Ox%2Fkf5%2BijkIRcIHOg%2BBjwiojzLfpSbN%2FUQ8CQLhaeVbg640stiENjH%2BDGXmIvo%2FrxV%2FzH6I9y7URt14G8rpUM4Yz7G3sKe5aynugGWSSQyzCip1y2OOqlccqQXoKPR0bKA4ZDoqW0ZUV5qfw2pv30Uhr5zRYm15cmzky7c0q0pvTqlmHWEXdwf4JfERHhA19n9hDnUm1COg3kWQIhXXouSopeaku6ttSNsVVCfvo3zj2d4zIcgcPJHx2YVzLfYJmTCreB7yRrAxnVNuFLOrKNMal9%2BTswsMLCNwscGOqUBm98vdtqBwS%2F3Ntsj8Hnkcv2EdHLAYiAyl%2FVeqW1GUFZp4gmeLwsR%2FaVrNd7SBlhYNiPmzhBPAasy7FLRmW578j2NC4bjcOCLOFGvVOMe1eXPL5S5py2s9n9Ccgj1v5XVCw5v3ZWIKI37dZ8TlfbEkt%2Ff%2B5nGix86BB79hW2xMtmAtTn%2FNu2zwuEVFxPaLqDii5MaexKkpx%2BGQ8iDYbdQ6nrO4W1Z&X-Amz-Signature=8bdb69b5e6a819667ad9db5e3fecfe6642a379caaffdd61cf01c9e3d4ee6dbab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

