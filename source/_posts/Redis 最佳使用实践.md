---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOAO6SSB%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFR7tMqMnwA7M0YdOOj8a4XpyOtFdCBN15xyOEfvWzHoAiAfRqfNoLN9QkGlVldbutNFDHl6bYNLEPNPFn5p71msvSr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIM4tG4PQgMnUYs4CY2KtwDvBIz9NvOANP4ncToc0UX7Pmo5SPyRBcJsNsFrhvHNvLtH7%2BWY4c7KOksAGVxgp7CAIcqmhf9wnWvA7V9H0LKs00C8euF2UAewDo6X0DveMJ%2BuHD1%2FUYYC2cahVYsgExOEpFwJxHtdponnO%2FpYjELBFgMxLEUwTiteb%2BTQjKQyDjmemPa38Utq9ROD0XcU5nV72YohbtQsDjTCrFH51tgIhe1q2d9INpOYpqlckpcqqZHilTdH9XW5%2Fk7E8Z03BObftDLPvQVGRaScKOBYpGa5QyWVDfp5nXV9uCzKneY%2F1ubnlvi6ehE2OK4ML6azVG7Q5fFOI8zLjoWkg1KrSwHI8ehy2fYpmm3aJGD9YHrixbstafCJzV8xrDZh91wfP7RzW5NoSARAKLO4XcBmW%2BlV6zh9w4RSHoX9cPtJuf33SCs3eLP%2BeWvxN9qJpSFn1VH1iHt53pOke62PdgSniirHHrWudKRc%2BJPZlm3nzrxrxUfpV6jYLNgQX0%2B4mOdccGrLXQG8hmFZwv8UrutlwKTaxexWmIvoL8bzvpdZB%2FtSPltp4BSPGXl09KQDzzBacW2M3h0%2FX1yH4Az3ivSDUKLldg3Pyvs2DH6ih7tamXF5h5nUjPjfHAwQ6W5Z7EwrOmQyQY6pgGRvF0Mom3xm9F6c8%2FV56jmbk7cdeJGH2lspIdJ6zHp5bz8rsxw%2Fhyv%2BXp%2BPIFnhK9VtxVcyXPD%2BphuA0TgeQcpNX6FdUmaUoTjySftuB4ITJUdwExJFX9%2B6zRcHAyL2keFrvJfhXTs7AzQ0R9ND3f1kRHRtcExsx%2FO3a0tF%2FosC9cvsU%2B5%2FRxASEdqE4td6tVVm2i7l%2FVAyPFDChh6ZDs14TnQY7td&X-Amz-Signature=9bde37fd4bf9f622366dc90044c9f1611ccdc8809400bd0fdce25c17a54b1a17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

