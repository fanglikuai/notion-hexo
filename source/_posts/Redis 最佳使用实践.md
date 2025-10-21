---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZVABFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T230056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQCiFjuSe%2Bq5hXT7F9Ij3W33OIYGz09Wv2zzdwWH%2BCav9QIhAMlKP53VeEy83mh0iiTZsnNjKN1GmmFgjaMLlpTSeBHBKv8DCCAQABoMNjM3NDIzMTgzODA1IgzJAdfBODK77NvFgbkq3AMNSS4lAX8iuafQEtJVAPajUVzqdbuf7wWeXZzLtigPgUqTSTQO%2FGCJ6gIJeYvNNNFlw2B297iwVvoiggKKmxZlLD%2F3mjzSUaMdnkydlDQkOEDVJuzhrfo8sHh8ItRw04yKyg2d2pSYF0u6joXkqkd4%2BKxMqxx284QJzkp8vtc5GdtzA%2BMy7hxX1e6SILnThZQWE%2BKjRYNn2Z4i%2BIIHIf7ZSu5EvMtYYNEZwnovSpBmlEyzsFySfVgUq4st3i0IZ%2FFxEey10JfSzlqNEbzb4GRL3x%2BK1LM5WbKv%2Fjcyo4Eks4Ph9QEga%2BaSPHGd5k1FLKm7nMUGLEOTqj2CxPk%2FD1%2FW1X%2FxUwXZD5yrOnz4ybKmrPOHPEUsHCv9ryGToU9%2BIDDYZqXpB08dzPcGD9sAZbTTatjo0Y%2BqXsaEjcvQ91EWOCXgeeM1iIOJ%2FtTcwS2YxTCnfHOgdeQlHSkcIDgGC8OfSx3ib9%2B218s2eA1O7peCJQn1sSbO0Gb1qZaGYmucS1xMYHFhjUWon9%2FRjLqfeCcm1AsKMdwp1NsaTNb8aNxasEjceOezQnk%2F1zE3zOn2I7Wq3lZKtV6BxFXmNtjtpuSaq4KZye1J5X2ssLDbl2T2tLzkprtLBxUhftjdjDCWl%2BDHBjqkARi0Nh66MHwE642Gcxjsg%2FsI1wmWLo%2BwgPnTzDhakOmPtSa4n5KDfI3uR%2FRm3rhBNaYTuvOyAn9wh1Xo45f3R8pypQnnytv7CLpbKocS1iXP0KIDPg2MJuUG0qsl5wobbVKibjWOVMiPOoWWhGcm%2FG3vASf4EHx0kZAlpbIOy4B7ZG6gFWeISFFoIUgHYVc80tfNPX10%2FWIJLXZ6oqsR8rhzcJqT&X-Amz-Signature=36e3f5dfad488c15a9fe45c80a05610fb551b7c5678829fcc02ad66a3cd10ed0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

