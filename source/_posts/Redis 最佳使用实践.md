---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAJQVQKF%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJGMEQCIB3RspOEyS%2BbQvou9OyBdPxvyDQBNRb1pTJVP0p0WVYaAiADHb0Q%2F0Nlv11S9Vt8d5rOpNt9c3OCl2SqSzJNCTPwgSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJLr2IJ2xzFHzOflbKtwDg5p%2Bt2voxJqnmVa6X5VAU1JHRuUCOwED2H%2FOWEMDRKr%2FGCQUPCip133ii4Vku10stO7d8TB6bvfneDvdba%2BuxKvxGCjc5soV6tSoWDnxdJ7hmHQWnQCAFhiakW5rIeeIi2nVs9rFJP3ULtUdjt%2FHLoOblgkAd0ZiajUH6wVDBpJKBa9MZuqQ5jQdGviDS7hkzxnRM5tFTCyL7GDuI9wxvo65UuQW6689SGsCXv4GIzxASJYIpiaAXU%2FhI2DHXoCjE13iaWUGCmlU04%2Fih186yud5Ii%2BGNhLsePeuGtrcbnFm%2Bo4XlOPIu80U1N0BRBLpxYVPvZnnFV%2B%2Bn5fxAik3o9aP0ObB3FAe67bbD%2BptB4F2ckSQhpDVtshmRnZ6okNIqIX%2FHDx6uwfnnY4828j%2FVakhZIOvQJz9tfYw99ER5IACTwHViLoZrc4QcLmoHl000n3VSOP3tzGQ2YJWiI4%2BZOB%2FFXh4Th9P6kPnqAQgNcAYZhUa1IFFUdTjXhL1YatCbcPk005SJQq8TGaUkwazEiTRaQ4xnEj6W8z9aR%2F0nvFVCm4PQ1CIhE8lq4ztfF7SctdYihz0%2BFcmzhdWY34WnvQ7xkawhqZb00LB95xrMWtWqXG7XxW9D4FCiGIwg7SNyAY6pgGTRreL4q99AMPaqJA8dnHjjcCnodaBTpgcBSGtKXRbDFt3u1EVW%2B30Ta76XVGG5wX2GDWMdHcROOwyok4tl22SM5DwrYpHMyKDZ7jc9npD3IHsRmcCd5H2k%2BJ%2FqvfF3ElUWUmcDaqhUwU7%2F7%2FryZJ4zbQE5dmVwYWulPgANSFCNowTxjLeSEg8aXO%2FDBN8XNvdJweEqVkLYBX4xGajMHPVYkBAxPNu&X-Amz-Signature=a0b13f31f159ba16af3e2fbd056ba3272861af96d30f307130a79687359e5afe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

