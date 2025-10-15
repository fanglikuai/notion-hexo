---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHS2MXON%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGRvD8ENrNxPCfGZ3jYanl2MIPtM%2Bds06x8QqwETIikBAiA4Dz3OtNs1hDiWiw7NEQ0G9LjV0abiOBqC9iBDajbp2yr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMrvhQ3lk6IPadb9QZKtwDPQmelvNkULQgE074J%2BoKDh9t%2Bcek2nluesalQ3AGtQVE4GT0qCFsb6fuKHSkRpbBTv4x%2BrsSuIuyV1M7fDHlE%2FYU3WeCwh976YodBnLD6HqmOkoShFPrIB9SSZcDWgLXYZ8%2F2td67GsoGRKHv4ojISk7HQUyKjtq1pMbYUxsyeCHCi%2FRRc4%2BPneZiNfDXHOQKSwBh0jL%2FtkSu05NDrOa0ODbI8%2FDuxBFGP8%2FDJS1IybqbLbNbxaaSnTsxzYt%2FDjaEJfdVqiWx%2BioDXwU4MRlJWTyj6RuLPkxv3P3lRlQc4DGiKPSDZmiKkMTeK5VH5P6vB8uKdQui4pQpx8mvq3tRxtDwOY9IbwBaw%2B45zuJbpbuDlGEjM169TTPe8zIbymxGyVDjH%2Fa1cuE47pjsniDpwdz%2FTrJa9QW4rtDOKuEpiwPpM0yWRwzYZwGhEiRrVWggEPLcPg%2FLRvAfOnZdF4OW0iRXDm84uzP7z4rkI0EQg0UUZBsI4zw2RYyUK3MOiwogVCHkUD%2B3heLXglVErgZwhuQv59F%2BqB3sfOScJiub4U32MfYXBP9rLuDV8dFUgYwkZ4v7sRGxZntK9fYIYD2tta7gchKv9XxHS%2BOrIGO4zpf0fj8bzwJPKo%2FcG8wo4m8xwY6pgEvN1trGwrmqJtliQoimO%2FlaykBXQBRAiQUD3r7xD6rqE6vrNdGLh0VpVTKC%2BrZx0MK4lbWV%2B2bzz2VpB7xZi2quL9%2BUsF6vs92HIP2DhLqaApfLksVH3DZrsrM%2FCvtQcPeoAzGGAZ%2BdcuZOcy%2FNhiMpzS7lLpF6Eajpm9BlQrfjB3S3kpN11jxdRqgW4bYoL32dxHOgqu6FZLdQQD0SFgW4KlqVobD&X-Amz-Signature=4b9f46bec9fa1e1e01a8e894019bc167b63d5bb9dfe6f56fcfa470d3d9cf2713&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

