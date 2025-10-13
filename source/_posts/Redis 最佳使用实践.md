---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632JDFGEU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSxauumrTXY58aat6UudRvqwopD9eIsI4ObwwGtW%2Bs4AIgL%2BnGxgdoiXqQXyc5cHQkwDitjB6l3pu42vE5ThGyvxkq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDAru7WgmOav7mWNPfSrcA%2B5%2BIW%2F6i8IxSYspL7wD85cNvM4FSG1j5xmIKIr2DhtXjVjd62Ftb%2FMSEGewLoy%2BC%2BGLPSN6qWXscQMPEq1z%2FV5lShmQPtRPlbQIfFh9ruh4a%2F6IXJsw2c3NIJP333B6%2FuJ%2BIEWYJRltAsQDV9xcUXKZAhTOP%2BlYujbwfmCGrdlXQivMCh1ZYwWkIDD5S0CSHnQcw4fhvduCW2UKmxaU1ki2SvMDs9j4GQ%2FlpXIioAE0ije1wFgX7VlAaYruqYyVtPcYkcSkpTeYgqlEUMzFkR6%2B%2FGEiaUlzgkT47tU9qxh2XwrX9d2DmarWkKmITn5MqHIhkEzHkMLoedbnY%2Bu4zPKtSQL517P%2F5g2TGqxn0mPuwoPc1fN93aRnn0K9s2ELNZsitqtVnb%2FmXyBnxJKLKcnwksR1pCi1bfN3LU7332fILwuWR1Qbf4e3ZrAZQjXaftdmmCADFBJcI3uH%2BtZ7IOGT%2FTNSqSkDUSQq82cBtaVZu8uN7G6VOeJuPxHxEkSUe5lRqJmot8Yx5B8QaOPJPuQHdLWPJEx3%2BwwICAcEE%2BuzuARuBgWvmn3zXM76hA0r4U%2F48J4YRhfaORetsctoqNL8%2F4P3qH7X90fp7iH6cM0ZfCYMRXtH%2F5NmMV9rMLjRtMcGOqUBIr%2F9eFOT8HWMQfPkHkFyznTFEGdryahpoh7iFAAlNHZ7JIo%2BoagmkZF21tp1eMYICSLUBwklx7ARUpAjn2anf%2B4ZLUSWrU68Sa3ey8d2sqj5F8rYDtvowEeJ1lHrhOpyzsyAY8ssKqsf58nJJEHdi%2FkKIVk5TdcovX6TGDraf6sLiQAnwiYxjNZw9VV5Z%2BMnpMpouYHo44SoTx28Gc1AHAAHQ%2FFn&X-Amz-Signature=f4d9c37ff8cc5251e4b871964883324b67e498a774e1763ae2b46ec7ab481ca4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

