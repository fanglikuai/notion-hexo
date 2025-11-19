---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHFLRMSN%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJGMEQCICrJ%2BTS7vsk1QUiJRkvDmvxCN2VjueIsINRiBW%2Fk7O1xAiBBsFlyy3K4oYMHobZ0ClzPBhyUy8VkMJDpRZezU1JC%2FyqIBAjj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMzS%2B5vSxi9sPUHMpKtwDu8pj2EBvj02APmCyWQTYKLO4Rp7wavPcqirLX4Mb4OGvUeGpBEXdc2DM3091UVWBiI4jf3gzGT53%2BpWDYjieU5hPM8TXOUWLd0QiHMi1mLXRo5OxVjRvAoLZvPt7AsHmOPj4sqQdyvQO7zZpDndsa7ZDCt%2BB8VMjPjoPTKnJPMTcQvo9M2wZuWSJtdRDGPo5vEJJA12cXUdLqBTQulsqDMyohu61XG%2BhJSicztICepIpyB2MNGTPkoeIcjo60JWF%2FPAw4LA6WMgND9m8EyKrzhGOtIroIrCkxlZKVuOhXG2%2FuWVSMTqVQFa3MWaY8HwrwiVIwEJixzzYdUcUt2ZlQH7svDnPBKprH0KUgJn8C%2FBgDH7seyf7Zk%2B0iMAhgUlOZ1ZZQlDuBcccMxLmbQ5kEUo%2BqWlBFEZ9gJhoj7YZNzhpQrgRQEgXksVZvD3h%2FClzfVasqy%2BnK6%2BUeTvOyeLiQxHtYgQsmrSRqGXTEYK%2Fsz7hIvKREIFVaDsr6m%2FTlIbFzjHtgFodHmdrzr5X5ElkywDCBJEHE4Vx351c4x2uA4tBT7ePLDTDxbDTynx2N6tfx%2BJLfdDp22Q07DyJtcmIhWaGxbGfPQxHoCGNOXE%2BQ2ICkEhUpqBuo71MEXYwxoH4yAY6pgFW9DGVxlJY9rT93UrwCMH3LCtntbMwkhlzdvSDwaJ%2BQowJnd7in1tIQTW36CnhDVUMjZe4iGMxpl%2FkrOmmzbNL%2B%2F8ORRPpL1l3OANsXrXAgnN8ACNySk%2Fa0rfXKUW%2BaWGuMs2Eh2E5Cm1Np7mjxVEgWbBubEbb%2B2%2FhcryIEYd49tijkY9c1I5eLRjmWGNvMs%2FzqexcByx%2Bmtog%2B0i8ZzcXg2ihBv%2Bc&X-Amz-Signature=2a79697f106d30d4215104b57a6c4fae19a657f43af432c29e4236d3c87aac5c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

