---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666J4HTG7G%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQCgw2giCIrRCPNQsh6%2FwSKewqSd5ATutvJD82uuv9KZFQIgA5OD%2BFCXsUuH7fbFGU%2B90SDK50bLEGge2K2HRY3YwGcq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDCnLFEv%2F6NxtspWddircA0Pt5eP5d5itub3XlNSA5sDZrnLjNrxnc2c10D66hY5k98Y3Gi%2FTXST%2BKgF2KealHMQpgnnXeqSv8bPJJzT4wdoBPhGFXB7c9%2Fdg763XEoS8ltTnLu3GjIRPXOp3UzDlkZ0cLt5YWHL1M0x7kz6gdftKr4lmg7P8ByKRk6V9RURGgTs1jSQxMR%2Frs7Kp1EOyEloLnWRsro%2FZKCo9qLA24emvqhtunufr4sqDgahT2fnpkDr6tMXSBwvSWoCXLTBG3QqbS5i7FPSbtvm%2FvFWlSo5u8y5Md2ZSRj9aJtYLgvU12rGsKiJwh7MlUu8tKM9FMitghIPjdnvSWESU3UB3%2FI6YTZxSSnlOqC6avbK6gVa5lAAV8JF3ht2w0Gl6EBv1b08B0MmFwwhg%2FfLfvBb%2F8blsvPqiFW3zaFXgJMt3%2FCsjDsa6qIk4HabE1hRvoN%2B79I1FbphKsp35mPkylos%2BDP4K4c4cj0qlhJqq%2BW%2FluyXlShziIE1FVWlUtW2RViTSpGHGuQ3RxJD%2BKFMpLbXP2OGpZEevrtUxw5B40h0Q1WEbkaQqUPGvmIG2a%2BnhmWxGy6oOeKvdAPm6Bddv%2Fx9wLuoUJLyL5R702agmVMnQ58aByME%2FrqVnACh%2FfGSVMP6mg8kGOqUBi7OjqEEYQOpHGstKA7vEIHfcFelbqgHpTEY8yPKj23pEa20ZsaQ32C%2FR3QzlywuaYN9lQhgHxvjX7vVBBaKPv%2FFAVJZNJuskVxWIAsyUFUQa7xBR3w3v6k0xmQyan2%2Bh5VWJksmUlxpC25Gqimkz4NoKnecH3aZB0Mc8Km%2BZjHQpsykjYZP0mx9sM3NAj6owB9ubOY2gdbNQ%2Fc4g4M5kW8EshntT&X-Amz-Signature=f9e0e3f1813ca12b537e5496fd07ab70976e7c0b927199e621ec70a4786da228&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

