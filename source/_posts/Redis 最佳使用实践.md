---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEK7UCFM%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQD8JaTz%2Fj1soRzifOoIHTWQkB6Et6B4zW%2By8iEXOkRxKAIgRVs0PI1FuEcgE88iaJ13HP%2Fv7gG9sAeTrY1lvN%2BeX7gqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP26T1eDy%2FLsbNl3ICrcA8bSFuZC6MYe8f9Nwvj792Y%2BhPS%2FoO3O5g%2BP%2BS2%2FuZfubXPjWDXgGn%2F79kdzSNHHjGjP7HEHbgo4o%2BI0WTCxZdsmVCe7g0XFbYHYWeosRMGpyVoLvfm%2BGeP%2BP4cNrOGXoOoGqr%2FrNgfjzW6BmL9hx0edN1ubrhFSFmq1E%2F0lt3ReEupVFEllbleVyWKhi7tBny%2BCJSIfUG%2F6U0vnl9a94qyY8aT5z%2FZHeuNoDM1vH8Y2HPwFNH29FPkBeLiTsAdX6hTvEgc%2FU3RupOnWJOXLqpYOaN8xHnvZ34X8a8h5c8ZNojJVbPTf57L4DXfNDlU5mZTBBW5gUUPDuqAQr6RbqoI4X1Bq9mVF58Ek9qLgXfuOWv4AhQbEy7n6g7HCKNk78%2B%2BL5i%2B9QPr2zylz666XtRdTDFWhH6Bi%2F6CHcqP7L3afVLF7AyzCFJ2vuuEZIwBH%2BSJguVNuFLC3EwMrmd6BSRcExoGED%2BwzJpmrR%2FrqphGS8NukVSV%2BD1uQp3jOCK3KCSXfhPMB77KEDkUM5vWs8PQrb9fpnrUef%2BQAO%2FRj1QgxW3OA3JlLw%2Bw3voZp01EE%2FScqgcMFoPO33DsKgDLEC2LoujZ1Yfz%2F7QG160KuPSA%2B35s8IsU7L1jsRZsqMOn2u8YGOqUBpShpbIwnWJkqHrJFKNT%2FOrcN4S8l14FH3ztPFRpWDz9K%2BGsSDh0odpiOrrUhbTLjeijklLyNJy9pW8kCmZeMiTqkn6OgeWqU9YvesrZFaJP%2BTNNK%2FWykn42TnFEL5psfZ2WeQ0%2FaHQ79xSRwYr5%2Ftf8HFHdq%2BmOCzuTnmTD8vh8wPVxtPjNhb4FUSxvjKy2jmVZJLggUofEl0ZDlbVEtgJU7c3qC&X-Amz-Signature=7c38a1c299ee0b300b940eb0b876c58a52829ad0ccc2a578030e04fc6175f8f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

