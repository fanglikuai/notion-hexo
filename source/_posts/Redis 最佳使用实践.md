---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQVM57FM%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbgk%2FIhtlkA47dRDkEkp18EUw9ogsvao4pdjcljMH9DAiEApBRG%2B%2FULP0KU1Ja%2B%2BTO92NYQF0uxVso6t4%2FPl1xb5MYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF45NhqdlUwUA3W04yrcA%2FEPovxBCn4y07Z0UIz6ivCOsbDuOuFjexiCU5pKywi7uKStfOa2MyUbQlfs0UErMweu0YCdZNYpUODk1XAffclPmd9iZ3Z%2FPQQZ0IOPgE11dW9PTGoxLyHZWvk48c1lwdj1e7DwhNGdI0%2B64L4ZU%2BguCTgKFfQNQWcEZBh8mDAoMmf66hfgV8Nxjis5ry8Lxzcv8Fcb%2Bg7S7ndcDwDeJ2qS0W8k6XkkNF2fWUcWJYsbGrJaEj9tK7w8Ci2e7yamKzVZDNHy06cfc3CWHD0Bqhx8NYtVhp48zg1zvOjPqNQA%2Bk9fpQwzdKKDNohNZWdemEFq8czVu8y0cqjYS%2BvJHoPLlT91S0%2BJu5GvdaaowHSaukfEXhMEqya4RrmUf%2FakCs%2BPM34hlXABhy5Ab9VCNisp1S7kb4HpwzzXC%2BNO%2B930ESyvKh7UVFyPTOYB5L7RrGExWtticeR0fhMiL1a4bO3pnvn4m3X%2FdYOZYwXnoM7iZoaX8eP4KFZIyzE63d4kG0vQXcG5KFnK3edrC6bHGm7DP35kZtB2pmATZcYSfkoa7pEfsWiTjo1UJZUcRp2F5r%2BTJITvKSjUnkg5h8QQoxhJmzUU3J97hDQWOou6%2FSwQd%2Ff9ec7w%2FWJn%2BawjMIeGrMgGOqUBCADOKsCn2OUvspJnP48%2BV%2BnCnHEBrSzymQ%2BCy4ufrCkJoHoJ4T2T5d%2BjG%2FUGzIpMGcmNnSd1u5Sotp0y5EwTEYV0a8h4Hx7iDRvBy1r2PLxYpNn9qxrQa%2BKcGpLJMefwjD72SQwsuwwL0SV4hSlT8VLsDFoXaYqLVfuAUVWKlZ%2FSu5lkBx0CYeNaOc%2FBFj2BPb7ynIdHvV4paElNYiVPyWa4MNFy&X-Amz-Signature=ab3af1e1fb34fa0905651a260c9647fbe4a489221dd8f658b4f86f8d0f7cdd25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

