---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJJM67SI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQD7R4T1Gy4Ng%2BBFdxibtQjzwhCJVIlKU8hCIK3dMAAT%2FgIgeJzQ7ptmZ4Qc0%2Ffiw4HM1J%2FhxJlRZ%2Bm%2F9YxrgghX3SgqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbCgEVmda2faL4PdSrcA5RpQFQgWOopwbwij0D0l4y9WKhq%2Bg64ztzLPnZP1SZ9QkNUfdglpMsKgNi8llPKoIqbMVo8fmk%2FGQTtKvqbo6yeWtxVPSnQV9xk6IgxPFZgCt9MjMOYkIThTwGUtoJ4EOqlgAeSdNJmdElX79fd1GB0dhqLSDOlqJ6MN4kf5OU2kkitn0PMOxxn5uRub7m8F1aLxu4e9vKmf6mRI51xxcbw3WKhnVmWBCFU%2BxU943bmNqBDB%2BeYlUN%2BvMVg5wUcxhf2mObAkM0uuImbuSIqxosq40%2BO7tTMTNWFx4MrhAQVCNxxyA576z27uSFGMUVu2eqqHaB%2FaG8ZtnEwhBLHOpkdtApC7qmKnuUcAtrCGy6xEBisXg%2BOoecA48oE%2FBKB7m7pRtGYh%2Bpy1hdD3XXEItVz79CdEs%2FJA%2FPLxdWNe9tp3Ey0hkjwIA2dBXCy7WVH7ztomJus93T8K6jD8pgVt86ayzwf4VLYzIZT4IAmx8fyeID2PhlY7fgyA3e8UG5nVeE86LC9MIMhjsibCitCC3ZhhqR8AAK1REClAulT4oF%2FgakyijVaX%2FZFW1yUbwNwEv0sztUU1BpNbvanLH7Yw5YYUFlwt%2FsbrEOZog2%2F9%2FuKI6kdbtEbYK%2Ftkr%2BrMNWkoMcGOqUBL6Kdxdb5uSAJm4G7WH4ortm%2BdHFxG0Kj9BGpfu1rSZofVG68WePFe7f7bMyvJmOjFJlNBokT9RX2gAz8r%2BN1PxzYTj%2BEM9Ih%2B8ldHrlNXKSanpwIkMURHqJQfy%2Fs%2BDfbMcnmpEo0ux7wfO90hnyGod%2FSU1pM1glYxoJvsz7EC%2BLHBxDA8qprdT8Njl%2BOquCJaCo%2Bw3hdzI1M0u104vfKQk9Mg%2Fw8&X-Amz-Signature=53ecb04666efe991790dcf06d525783f5e5fa07d6524e0b428b3b82857baba8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

