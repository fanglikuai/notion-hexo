---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GTQFXXE%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz7RkcmxI1zbhFiIKuvmCiBBPFLm%2Fd%2Bv0HPVMx3iZw1QIhAOq6xwo7GtHp7VyQVzr4vNbv2G33VXQ761wcsuVsWDVhKogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxq3O1MYdwYOgjHMYQq3AOvBk2ghuSuoiaDT1CqKHsKPTaWUwvZrqeJ4BJECFeOw3crouBSIGfkMIs3a7v8vzdvsW6GO6CumUbJWzLO3NCDKYY5T2BxmsjsvlMECNfnOqNJl4v%2BDFeHF7uV8NPEXuFfSm1Q5gsiMUOadIlS8eE4T7wrtDVedIc%2F0fXDsowXMDsi2UM0DrWCPPI9M9wP8Htl22VBnJQMuyAVjMhY7WKdKEzhF0pOUizXivu5tC%2BgESdVtHuQ1EFuU7qUuJgVjtaswwL2tHFQtpmjdfIwrCTcsiyi%2FNFu8iRuShRgSUwabj9dw5sNfYyi3ezmVChAoqWLNoS8tsE4G9eU3VS1sRaGZf5wpLS8o7PDsdkphrmXSoNj8WJOBn9qKnFzYEkmDlzSRjPCUAapAAjk28BDBWijq1zGb06Ib6O35XFV2o70x2VH7lMKmegN3N7yKOyyXptvKSNc%2FlFkaVG%2B72mFm5TIabP4HrvfeW81PPlITELHpqmD1LNJ3J3iZg2YQhomB1RT7Xnfga594d1DlLWLnQxb3kHvv0WLUPRVxsf%2FUb4xgBmaQjupAg6rI95u%2FLKg9Iln1X6dD1pjNOizSisB7IySrfeWSYy2sb%2FtfxhdYr7uUQgociUcnigoA7%2BcJDCxkfrHBjqkAb69GFkDdCj0thxvyaABol8LPO5SwO4s3TD%2B3IxcNduj7mSPsazKLUUCEEoBB7YTovkjg%2FR9AJDF12T%2Bp1B64rdm4aCmLpFkF%2FZYO6SSyXWiKHYnsMZHBK4Y0OCRr%2F15L5bQJ9gLFeKMl4rckdwKkEGC7jpmNbWij5e5jioKUtrBTtLmkypc%2FrLc8AhwpZk8wbaqDQQkiOYfy3gQ5jCBxHUssans&X-Amz-Signature=9388201f93d5ff213c3e5c3b08aa34592793e64953d702e54ce5fa5763b402b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

