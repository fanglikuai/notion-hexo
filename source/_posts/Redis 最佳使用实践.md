---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZFMQC7%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIE%2BxShgdyLC5mWWqB7Dvi0Li2GP%2BskNjWNOFKaBcrEOZAiAJqcv%2BBmrxkRojLzpruqXaln0ilJt8kL8t7%2FwMhmZm5yr%2FAwgfEAAaDDYzNzQyMzE4MzgwNSIMt7Y7vpBBxWN2yAvHKtwDfYnroJvP4HSTtgEH9w7ifcndWfw4ROSgFFDWN%2B%2FK1VMWlOnM9SGHEzhwIjWDWn172lvUmw8Kj72H9WY1W5a6xqrFh1qOwRf9ks%2B982JHE5d28f4%2BfcRu8%2BC2ZLymG8fCIJsqYvmlxmzsHvWotoK7FrXgl6Y33%2FbH56rkn6Cf%2B2pbKTQfXd8rszSLB9dy%2BAZt0wSI6oEqDKF5dapa6rOMBJqCiq%2BuWvYXB20TIoNMQYEX0cnNJ8tEbD17os6QVoDYHcDE%2BQyCBss3eDbzXqtbkA1Y1SxyZZu8NI4GN8dfDCHNxo99yyhoE0LGut1jv6%2F0FAFt1OVpLYu%2FGSw8%2FzeBq36SzkIQHm130%2FcvoqeRy4ZKdFqEazs6pFM6X1FHdXZxyG0OOOIp9AazE0FN0kE%2Bzzq%2FRv60QmUooZNF9topqP4p2Ashc%2FNCeHg4k%2FvomKXutyBT4gxsXhoX%2BBw7XgFAZSQfFXCgFB6CaVB7yGw7PTewDLIvyFDBgxxK0wYmQVbL4JqJ2EVxx01gUEOoDTw0gmUdwI2Y0lnQ5jy7g2kyOPUKE5eOz%2Fk6w8lmfxbSLCqCF2VLkcezzo5c5MB7j7xgrtX1vffFpE%2Fv%2FmzvTdPeDYR8m2HwiMZXtEJYK8kw1KarxwY6pgFSTMWUt5VP3oGX2%2Bu0R%2FWogtpxbmRANK%2FNdtYbP7nsZ1yRAi0E8t7ZUYTg20CAMelXxwN%2BHX3P%2FPNzw5%2BM2OXv2opl00JKbvM7hpcNZBFqi0UBSpHb%2Bns7pDAVFGo%2Bd%2FWBRMAwDLrNxZMAU6rGgUcI80h2DAjjk2R9YQxFyVdvyW1deBbrNd8Vmo73tIasN%2FdagMTzVh6X0OnpEWNu8fstJUHdpJcA&X-Amz-Signature=029d51cccc36b780a2c13e1996da477e388a5e2e48b18177da8a281a253a55ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

