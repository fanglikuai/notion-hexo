---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKFY4Y4N%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIAQeQ3j27OTiwC0nldSCMpjdju56M%2Bvz%2FhDmBdOmowVTAiBJhEXE55QKLozFzPWWso7ADfKE8qMuwbs44uRjYE7NlyqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyTs4Rq1fhk6DpEVqKtwDSr34XYZrmTzLDcra7VDEkizkixxPKJBIzzDQOw%2BR5hfgpDbZKW1m3jXXUmDjsw7IllOiWe3DnXP1zOKywEmiN11HFYv5Fc1NdXqPjOZ6zSmNIDmlPDeFp%2FKx1pS3VpvTj96u4qXxeMpTBzNF0W62IHYq9iCmD8%2BsiQZZ13D24mQNusPobxiiacWdA6piOvNn8owAEtRvLw3HvAXd7%2FtrTyUBwszYZFjmgcQQPT8pwYUHSjtcMLOtbPjMq9mMim6Ja1oxQdVuMK0abrYH9ruTkDpwtNq%2BA5Mu5%2BgHPrMBPQaIus8a5s4%2BKm7bEeEbdTQ1jACkgYtXzlLFIwWCxgTMZgMEMTWKKq9TezthUBgrh54YMe%2Fkjxmu4J%2BxdrH2iZv0u0kDsQ3D31l6ntyPtwICFdR6MAWf058%2FChKCcUieH6%2B2cZyZsOz9GtKFMd74zF6z3kWXwEu1jQPDlW30TX%2FqOB5SSUditTyDAxHcYtkdc9vhizqzHjWN5WO3LXPywqTLHvOaycUi52if2eWRJUHVDyfXIi8TVFFN8haoCe2awhpQnsGmF%2FeW1jUNq1QpL6ip%2FvHgX9Ip2N2xnSK1eeAdjv9uk7futjkr1Fdx%2BDZ3QBos1wE%2BiemuKNDjFZswmYmkxwY6pgHQv7G8zDYZrNJkuIkuf3oL1lkzSGlLUtlPXu0MOtsLh64TOI8svLjNHD8zUoAyIoRsEaWg1o3YyZzjijRQEfhQkZ0LePhiyB35eMFakmzY3yo6kENs9uA9Ppz2%2Bsfrh%2F81dBcuK%2FeMuhjgaStjBTwcWfAW1FMhDSEFPxnbgQExyY%2BwUluryKPxAv0wek%2F0Rr2D9zn4he3%2FmJoFgbjORAiTmikUvEv%2B&X-Amz-Signature=df16c6ee1265a7706610e06e0657c44e7a418a7626f57c52f6ac1034395abc0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

