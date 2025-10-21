---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHAY5CDD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIDMZYCBmk%2BSG9TnoKCDYQzaSdQkD1iJqIJRNnPsxBLjzAiEAktKzn3ZsGIx50c5Vdtg540%2FZ8%2FJhLhf5JkMw9SzXlA8qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHU7O9H9HNk3y14CxCrcA3o8jxr%2BqvTv6%2FzKr4nso8dVjRYVjqGKVenjVN39cLFdxoae4%2BHFljWSv9JWYRxp2AAz%2FrTMkCdwRxtscMleEWpGjJIJlSmogeHan8aNsqDHjqx6A8mvAd%2BYh72unhGyQcw9yYLQ5H%2FEazcDp5QwsMPRQRSQt0NblJbvC9EkphLsi0nahCbdE7x3ZIrib6IJn6dwb92WLhaIsSH23MY3U7IryAyavbCjP%2BJYL3n7z%2BLq3eDFcHf5Ij8P5aRgsLYKWuD6kB%2FhmLgpvgkjAW%2F%2BuBIK1i2dJJE7mFA0aNvGat7ODkpJAA68xE30mcaf9Iau7DcVP7lthKPrMtQrileGfUXB7Ym%2FWc29yf8u%2BAKYBoRdf8zCrq4GRxDt%2BT4vzE%2FIxI9k4ug494gVqaZS%2B2kRVPCRsppgS9kpQtYQEtFhG4%2BDeJQfJ15nMoxMB%2BFWYLdAkzaGwAOs5peDtwDHhOIxZ70xviUvtic%2Fps5InoyYxgSShp5lRahIaiIIz5%2BKiymbClQz6uAlw1VwmNikOyfqbeQX%2FYt3LvkVdtzpdN07n807m2OSQvOyTEiPtGi9lbBsgEnC61DEfoCAkIPlOqMdSaBSij1Hxs%2FAHDX%2FeLeIKB2dGhGrmLimVfUVtrtwMLjq28cGOqUBzlfHMEnKqGzxlNxKDeKrTFTCTvaRLLSJxrpckTBDDcQiCFasP72oY%2F7K16rzoOH%2BtywE%2BeK3cN6Jd4n1%2BKaRRu0Mjao6pmGerWsGRIfOH2%2BwthUuRae9aGvRwGlqAkvq3DgmSc10TJCB5HX0YlQC2sbDZEh7JZkZuKDFHXZtmUW0FPgYkfN%2BY8YG8D2W2CkdLJQnOJ5BrUXUDrDTcOyMQ6TWv1HS&X-Amz-Signature=8520e2a9eea90d7b6df480881eef79c30c3276ce43f8c1a021239e3e662e4de8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

