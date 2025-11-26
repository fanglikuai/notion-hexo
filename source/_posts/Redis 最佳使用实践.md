---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUWAPK6G%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHW9IRl6ZdAFuXaBJRtrxYagJ4xpcbLR8GssiRwCc70pAiAOKCOMhZ5LByDmarB%2Bkho1XzEJJ8ew1BZE0BdOfk9c8Sr%2FAwh4EAAaDDYzNzQyMzE4MzgwNSIM6RIV2dkX%2BMRDCE2dKtwDsQeIlnGczF4%2B%2FzbMKF2ZuL6O2HgkVgHm3aAgdgk%2FW0mVC2mE%2FBNzPatK8tKkFj2bxEBA01sb3bTnaTNEySeDnjTz4YprUCBAJ%2Fvxs0%2BsqbygbkbLWLOKVvy8uWZMsU6WwRCsz2I4nSlpDRuXU35rYsszFt2J0qJgj4t2qncDswuoQmBm6YpZWkxWaleKGwdDter66%2FeR0cpHYMfnqUAxJekMOq2lrHIxcnv133eBftl6b28J556cyEFgMx2bRMEWmX4GkgaisOuHK4aWOXayBc6NRf0JKOk3JdPFVj4uu1Vr0RRkOLFDxdApbOEu3Hz%2BicRIrvhRQrJvLjYv%2FyK0zals2WNfeazy1iCsAAdjv0Q%2FBQSgDZug8Jw1K099BPV7cEo7%2FjjIBXRR9XHH7i7awThoodKyCFZhWTJ7F48HwtewzevmwlmMerCBRGKZKxtRv3K3T%2Bhpi5yf6Vw9zJniEFyJKol0KFfqOUw%2Bz2GoMHX6sMpGrk9YAoch8%2BHfqDwHGVRAvT48Q5blRWBSqzhEX2Cm8AVzalh3lj7EUYYW3kkjSdAEK9GLm6mfIQIG6oG3Pd8R9pD3TYjcIXGy5bYqRt7x69Z%2FkGItLaN7ZvENnkwFj9e5Tf6YzhKeg4Qw1%2BaYyQY6pgGUMnYmwvjxAy4upsctGa9QRs6S7wpONh4BbiMChWgGFahMPy0BATG8D%2Fmf4%2BQ88BUnn5zu6UFHgKMNC3Hl9h6%2FuhxAG5FV5olrtzb2e6B62ENHFC6pLycWLso1w0i2F%2BfZA8dsgpVCoa8BjFQk1BMun2FUvlq76gCneTJXdw%2FMquy7W%2FfiULeR%2BhA1TW5SCPBt5wS5DYh82UEY7TmTFGtsiPWDZzF0&X-Amz-Signature=43c6fa4e59f731f8bec2180231341730a4fd6c131f9443c824f2db62bce2802a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

