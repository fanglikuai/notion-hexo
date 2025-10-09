---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI2PWPH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCICw3OpBZsXPhmUsyLsXODr2F9bNxTIMVwvxLpYHbpT5sAiBvP23lcOsFvQ2TV3A%2BiR6UnsDDOR4RfjQYxz%2F%2BxofKqSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7jWrb8yVL2C4z%2BL5KtwDPgN4Y2PM1xCRJVNbN8t0BvoHSCxVmT%2FEQUWf%2FDPRPI1xSzez2rhf59%2Bfhs3sUIb2o58KWUhfOJuxVmfk06sn5qQ56z9UhneApdpVlaqawIbLW1skOmNqo99TrhyYeUCnkKHAqYBDfHfxpiqg5jVhSzKi5PCVDTYyFYlAX6TKvdyGgMUGCtVe2krFmr9661TPjY8Q%2BhuBV0aFqz5YwdrjYq0Vl1hsPtaPF7TwHtPUM%2BFxenN6h5fRVRJYh3L%2FSLMqiejTIoHLpHEmGy7radmlaeEqkmuTLKZ42%2FOaOmlO3RwOyO8vC6pGgdf85cnMwuOS3m4pvy%2F1LRfKDpkGOnEsCAoJHNDnZfZ4lWzIcyXHMJlRVyUMVPi6mdXPUTQki%2BStDPiHDwV%2BhlQcGKhWQWnL7M8Xl390WT6ZKwkOyHlBlpZ2%2B%2Bf133b%2BDgd3bQrl8YhH2p%2Bf5bGEY5KXyeFo1Dk61TvF1ycgTsIaRcdKk2H1WHpuKTN0ZQshdKkz7Lr1rSFYiTTD29H4bYJ4lT5O0P77tjWLsO3I3nzac11kH9XvUBFn5YMd9sgHE6z%2FQ%2F2jz3yTYaYPZviM2Qn%2FBAqb5e%2B%2FHWo3JOtRTmOgmgK%2BGVcbrFM%2BuTrUoTtP%2Fg7iSmgwu6SgxwY6pgFtolR3LqGCwm4FdVwkll6XpqzyDDeq9ppuLZuKb4TQo%2Fi5Efr4ngaGBdtzqdECRLg8ItdgOutf4ofdecDJwSe2o2ZMHj4Uw8x4Db53rsu5zuD97ReOKNS6eDpde7CyHTzNllxjrdRq%2FlD%2B%2BJwL4IAz2eRjcwGV%2Fl0QV5NgBqVNX76m2Ay46mnUIprXqjDblNJod6tAH5ZQe5DgvqaRj7%2BLE9t47tAd&X-Amz-Signature=4b30e68e150a55b5b534b6cc8908f4011f7348368d8f16810b66cf55f64b7213&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

