---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRHIBART%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIOEvEByZT%2F2WbDADEh1pnrtzo%2BtuPzrUQJ5PkqHrP2AiBfkMOUiyAFGIvSxZ1XXtsiZsAJ0HFcXm8C9JAHj%2FfkhCqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv5y%2BGy1ru6T78bk9KtwD6Gbg%2FjykGXroOXyeINcy0mLliHYhdKx8q%2Bxt4EpS0xmYqq6Qdq5bAm9GmRGgfWySD9IJUjHAgyFcaFubmv6SnCL%2F6Zh8cgRJ7ld%2F%2Fxz%2BfX3WrdtXZ%2FAkKcISfiFLi9rMWohmI54tPk6tDTIzslOBxQSksvS3%2BA51QGa%2BWEwO8IGTAuAY3va7hIyk7cjjWWgkNnEaBQUU4Az5nxfY0Z3hNgRFQ9oqX82yD9TGFsWBvChQBdBgEuBRqW5lTycAHnN3p9YuLp1T0bVAJGbyiuhz6wBJ6tudNBh8brhLAeygy2iTsUTr8jDh0lmB3RVie%2Bnv6Oj0bv3g78mEbYzorn77B6u6uKZ8JxkqOTE2rpcLiStlmxaVOjBUpgWhPVrbqlzjXmM0FFSRwZ1E3NYHvknXFL%2FlCCHzuq3Z5WSYZWho0bIJoHkrKjxPt1T4JLM2OtUVxEPDvmw1AWQtVacf53SodNuw79eSGMm6Bnw5VswakB7q6k9xHejP004mwKtqUpThEgXHioM311rFJ9vZOillk8WpKPF9mWgsGCRL7xV0826kThIpNfuclVv%2FcSNqhXpZ3r4CqEX%2BoJNMOOHbqNZbOxKxRrcKua6YZW3jw7sYoTmeBpjBtRIndCbFVmswgp7BxwY6pgFAh79SSgED5GXoEOe06%2BO6OE5p64iSsk06PcqHNOdfeghMIoS264NPL9hAR8%2B8sJCHBkc35cU8cHZi3wYf6KZsep3mo1hEHAdQHgJ2Lt4F%2F3zLIg0jp8N8ZjqohHR4%2BPcTPraJnOw3sdONC%2F0%2B2EJ9qQZ6E4pLkougFuiI%2BqgGNtt%2B%2BuAHkdYpyk0orNcYJJi%2F1O%2FvSM4vLfWdYYlsO2L3AvKX6oud&X-Amz-Signature=f2446550c1bb7f10ba52a1bde2f599c561d373d5630c4ac0fe141af45272f31a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

