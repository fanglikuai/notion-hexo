---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XDVFXR2W%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T140046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIH0n7%2BVm%2FAEkP7sb0MWttZ2rcg4%2B0bdIHRoz5owTC0y%2BAiBS%2Bzw9OxlMrcBaPI1JrvLjhnGkLTX%2FdP89uTUBdLxlOyqIBAin%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfMGGF5uxG7VWNfAfKtwD6bL3Gr7tFnAbKDI1RZH3nJn8Im3wGytmuUaQ7oZl7PN37YY1zv5dMMbAIx2krr0CrShjfBzQrEfT%2Fh2dSkptSM63rrR9iGOIBr3hly8Vcw0KTCE1v%2BQpEaXwDX1GI0oZ3G7b1W%2F8fp1xlNGKNIusklhQ2kPJf2xCpum9C5AA9zjjD4lRtM7EwKVSSFYWBZdP%2FUI8HmbWlK8t1XtPghoYUzBa3%2FhqkmgbvfRE9v55PgOVH0jLUGvHv6hVLJw7%2FQgCK47LG0HPA1CbvsEqxSWQ3Pk%2F1xLGm9MfP%2FwWouO0M7zZiK2YRlkfbRZ0bYjTG%2FpuuJ5ehuuj0QJUd%2FsDEkTH2nNTi%2ByYfnamGJVxVW4PdOHN5G%2BkRHQQnIXlkt8ouijk7vugXdmwbFTs8RSBPYGC85046XU0bdjk24Y%2BrNjYETIDDec%2FLa5C3uhxHYQZoRh76YdoE4SQUciJnN1MENBBLx%2Fa%2Bxlbra4uMXVowpcZpn%2Fz40dFLN3hz0T31oi2g9r%2Be9xNWAY9h8mR9CefR59QKeAWdUD0ReeLvLUMwVVEcdNhJWL%2FkqGzU9NuHFkKlz5N0ZGpxb6topslMJradC426t2FzNVedXagGCc9QJ4QkapU1ssc%2BoT3r%2FRdZAgwj%2FiqxgY6pgE1w8XyPuaQIByhhWcxG%2B7unnMi6%2Fn%2FvmhKclOO%2BXLaWsbQ1qZJ%2FMdcdC%2FYw4zpMRVPoiia9BqOTEbeByJUQtHRYb4%2B%2BX4ZOmwnpnyGKX7A1uR562%2Fte8eBeZjs4SKcFi6lgpgyfoENrj6O%2BplVslwTuaW%2FLOLVsJLBVWDqk5ofDgTbMjnxOqU%2FDz1%2B3nNVutOVuvTh8nceipgSsmGbkfrEm4yz4Sw1&X-Amz-Signature=bff3f307aa4fbdbfb4d4e7de28b6b62eb93ce97a0a279659c770858983160d28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

