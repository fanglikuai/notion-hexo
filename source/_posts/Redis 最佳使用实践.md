---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KORLGAU%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCICLV9sjuVDGRECIwfIrWHHsR1lfUa5B551%2BETihFkL28AiBcurPLWel4osT0AszrNJu9I%2BFZH7Uwro2tVdNOIgytDSqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7soKfVXiQ5MrL4JxKtwDazwQ0pQ%2BIoEcj9jIHV5bF1Ex5cjjTb5xbh%2F%2F2d0yX9EZuNVpcEoNt1KORrNnT9O%2BpVlTfosROAABTLeAm7coJ1Y2S5fIbrUAk3ShXziEJsJtsfUJvnbKtLd0MvC1yw6T2%2BqZ1Ry3aDFnuyLUwR4OtYlua2dpPOwS1HPr%2FA9KKcj0eOGjtxob7e3Fih4rpK4bI5YD5%2BCvFqtJA0psc0FuRvikbdDaJJ%2BCAkJGRxVopiPKj4GGjrFfO9WrZTOvIUQkrVvX9fNo4qRWpOmvD4sxYwjyHACuJ2ZQ8bidWxDeyuQTJ5%2FRml9FW%2B8J%2FHaiMQ1fpH%2FBEWVZ6XpK72B1tMwmQLW6I%2FHBHztUUMu73bY2FxJFq3%2F73h2fX8NZvY0TqXb5Kuy0PzIZ24ZC9aIJY04PeKrrJt1%2F2xG9Uz1cgXtoUEVPiCspdJIB0B4p1S82zKrx04hWxhygAVYBLHzeob36D%2F%2FG2yBRgbvGIL2mWsiZwvUsMnb6tK6ytzVJS%2FPk6gN55BEeiVZYnPRbpZ9WRSgbcZ%2FqdWOWEZwamkzSfmNDrPa7Wa%2BcxLpGDamo1C9m51aTIDLU%2B8v1ZvGisbPmud11Ra4WeWONvOpTYPUht6xwXxJpXad5WOsm3IdeUnIw3qOnxwY6pgHMvKEtkW5XzWDhoaZu5myRHJ3KWgPZSkdFkvTvcbUA1tJhaVDTM3jgD3OLRtUUjTlSZbk0q96P2IBbAZRwTlt7rfODinDLdyGwoDO1UGsDJapxkSbRtnBsdO%2BMbjV6Hlqprs5spEmdX0LhKmc2CR5w%2BLvna2bWR%2F8C1Dgvy8LPrhtW%2BP%2FKsx9uUnVKadDNolSzdu%2BDWksvBamzbP6Bnxa%2FE%2F%2FIXVds&X-Amz-Signature=f17e2116c6e923acd40a1c2fcea23a48d17f8ad3be96e1e174ec094911823dc2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

