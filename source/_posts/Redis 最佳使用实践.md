---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665764J6DM%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiapl4dVjIgfu7dvqJ27HfsFVOIQjVmdmyxiE2JiBclwIhALWek8%2BYsQtebeS1OXmOmboa8EqQsWFXtPRt0cLnlYOxKv8DCH8QABoMNjM3NDIzMTgzODA1Igyxy2bXKPMrt8Q5N6Yq3APP4tBgEgk2wHAO75GhsVBT9F8YG%2B82fmwXEAYTi%2BWiOMH18DWff9J7EmCJCBAD2gzNR0GzZsAAk%2BkftInuO8tAlleTQRZvmGXxh93QKs6kevJFGFDrKCALsvnkg%2BNN6wQk%2B%2FJE382BTBrYdAEm%2F0uRK3njFA%2F5Qlru4ig2qeZ9hk544TOuCgKMxfj%2FhE5KEbvgMVhHUgmCv7RDkRKm8nMtbpiCh9GHEN1A6nXxen6UmIkGhGtMbXL3hu%2BCZNbQU0GYjL%2F9j0DOQtHtNKry7PO15TH%2BRdeD2ISR6R%2F%2FkSiJvXXdfp5tB%2FaL6eCztp9XkglJS7GDJXKQeFL0LBMIrcgku7BQcWzET4%2BYTRgX0HLKQdvGgqROVOY6YszjwYXkKL7wmvc%2BKZCfa5pAkegslPz8zeYoxddiHEbmfXF5LIprhNXTvW5Ybf%2F6%2FePcPNgnHNpnGcauA54nqrsx17zUMm%2FihJ7yD9sOdKDWH3GO9Z9dAqQzSm8%2BfQFV0x9XLemhfJVo70IlmpmFp8U1qNk6WglVi%2FcFZL09JGT9daOlOJcOhu%2FsWCvQjjK37qYUDsZ9HtrX4yYiaJMGG3ckRN3ygE5nQJCWXBQgNQ818DRnyTZWNcUvGs5lyJrCeEESkTCetMDHBjqkAd2ohZf3YHI5AhwILAvC7vmE7PbYgTlhMWW9T%2BDUIkkw8GP6bKqeJ%2FJT9G7KFXYMgfrom9OhwSa1KQoIzQBkTJUKHE4NyObdlVepauxky6ey8eOlhfl7H96Ytt9zOBfvQAaa2quEeEZBWQPflkGcnzkLe5nGHdHyRWqDwOKE4cgCziS%2FcDl7ZWbGxoAUC9UjGJJgYJ9kgUEB6ARrq%2BaWwsDOcYcl&X-Amz-Signature=9bc03030d574cfd7a99fee4c4f39ec33726acea54b5cf3910a78f36c153f0c28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

