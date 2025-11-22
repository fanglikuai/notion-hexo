---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AGXEHS2%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIEUDrShfwN8EHgX%2B768Ja%2BtnSzH78CWS0UJRKjsHcHgLAiBUpofk8E7pv2V%2B4Xud4B7ouKIFtSNUxqQ38ztV9aKUNyr%2FAwghEAAaDDYzNzQyMzE4MzgwNSIMZ1m3OK27cIz6AAw5KtwDX7eYGJi0ARzMuRPaMODG2eUd4E1Ge%2FyOxQOOHdS5NC%2BnqYBakz68aDrplQVLPSM%2BO3UdM3QwlERRRmm3PkmAWMLgsfx7pkzWfUPdQQZX%2FXub48hsBaHVs5rc2SmFs7kRxEd3KVCv3IbZ%2B9qhk6CexUV4Q%2BuGSVLsT%2FWkJbH75%2FXYtRgQr6KXyj8EBgCX1vGCs9zYJpTcc3aonZ2D4kgQu0Wr6d0D8u35mLKCEDX9sDcKWhewwQS7h7PWh33Us0gXUv15yOj0EzrVAZps%2Fz6NNGTBljgvJ7oRUthzmeqgbA9rA7%2FAjkMX76l74ObYql64PXHMG90VWzDGx%2BatLDTPcx38ZoancExspU%2FJRnIZgnzbeCMQzPTGZrxdFdJ8wswGJwLFC8ufqNR0YtwNVEbBqPNV62r4Ozjy8KsfXGAygtMgjG%2BLIFdaYBbR%2FmJyZDbOAE82HV32RuqQAYFVmvpYnXQm6FzY0m9DntUc9YPt1KESLl5cEOBVz4qeMyZNXIo%2F9mUDxQuwQOtu2cISkDDPhTVzeN52pfRJInLHjRT%2FCOnmyYoklHzTIlvB4EiYiomivLvRVw%2FfgSfwkMxX8P%2BWzh3bfdser%2FXT7NT%2BVxPy5V3lBPQ7t4Vu90s7D0Uwhd%2BFyQY6pgEYsQi73Kz5mc8DIG5fZK%2BMoEt4cN7XH7g3baxN0Lesf%2B%2FCIz2UdM5iFtyB%2B0OyOjhH05caFd49LWLs8AACJQBndMlutSRcKHh2GqJu4ZBlp3SjOgLQMHEAo3%2BKXrcWD0a%2F8VIVpYrdudek7OCjTU%2BdyfhuRGmRj3%2Fxtt0qGT7hvPs9ScLhCovXTlqgx%2Fn7uVdZYvHp05IZ8KON3GvOknMPi1vBlLOb&X-Amz-Signature=1bbe12bfef05759ccbf279ddde507f0fefc3a6f1ddf36197fe665843543b8332&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

