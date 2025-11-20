---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VMG6SDB%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJIMEYCIQDrjbSXkx4nOKv7oAjiwRidAlHh3HnUki43SVVKrYwODQIhAJ4C%2FHLAvSzFcrNDfV1mdl6iH4M9K2xCqsGYGntRhN6WKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvpMW07SF5UnqqqEEq3ANTCVhtiZ3w%2BEOufWxDP%2FHi0T7QfcAeQcLe33lGrpB42yn%2FhiYPShRDZOzRJRVqj2tfKiS0Qfn8lBJDn46lhwRvYpJJ0oe8knZtpoEvLP8wInrtWGjh0OHAsevw0XEbn7X9Vsh8C%2FgHEWzSAvxecOb%2FfcikIoWPXBfo2z76KilbUk5rdsHTRea11JXfXI0Uwa%2B2qfbXvORG3bJgSUl2TxxjTB6b9zvEUeJSKDNuop5lvOYxUacKSt8oegAaLto5WbelOFgBlp4Dh6w56u8LCEyrdQT%2BlmnTx5NB7%2Bk7j4woJZu9KUCytKmdga0zTbXdLBcQv2YD%2BoWBT14zDWZk1O9mzs9%2Fj2LeR8SjILpjE05SBcZ%2FX7kTblVPGC4BT7v5A5nEQE1HlTdjphqQFz3rxDHbqzs5McbUk1rRz19i%2FhdMTtvwSr4OEJdKcKeb%2B%2BJAEN723vPqVoVfdCvJj4mKzZkG5K%2FueENGPfsouvZh5rAsuyrW%2FtoV3kiv0M5dJFS064bw7cb4bHAm0oOC2z9gzx%2FfdM5rSPQ5IVbLIqiBBHDyXB4G%2Fxum2BN0zKyiotps8k%2BuZ7Mv9%2BGVbvT4oZDmOsubMqg%2BwzZigaQ6i%2F96ZdLGnXaLqSdwwYVaYSsz9DDip%2FzIBjqkAZBf7wVPzZSBnL3%2Fg98YInf0wvHdauAGkwIwM4p%2Bthfq9Bfo0ICjlJG4WXGxs7Z6PKRbn2pIg%2BPlK5%2B%2B%2FtFB5YWCHw1Mt4yXvG2U0J%2BSCkenbP4pKx1OcZqiYl8U0jzsXyaO6J6y5sEdkN2dRx0EX6Hxx7swez7CMFNqRdaLbOFYzV84SFLtRxXDRAtNCWXwVOBv1o3KpylTNY6ef9AMLoRwYnU2&X-Amz-Signature=302a762b7f424830dd4d6d302ab5d0fcc18394719782bff1b49d82fb3dc613b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

