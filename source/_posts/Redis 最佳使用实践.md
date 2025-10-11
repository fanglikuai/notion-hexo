---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCMKTKO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIGsBUnJjE6kMYAgltCfDkf6vf9GRemXVM8ElCDi1m8nAAiEA2JT99P4GN6vElkz1oGslhf%2F3ve3vekI8w7Hbs0Fs%2Fhsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDGkxngAW0zQUbrmdsyrcA6XD1RVPey%2FR8DXKtZIm705%2FBuat3TGo%2FcG2punQbQGRmy28nfSNLOfCCVU2BAP%2F5cGNEj5MAEr1VAfvmV86AvsA0kXw1ggp2VHH3I0A%2Bv3Fu3biqEk44gg4aGcz7d3UXXeUda2t29qrUADLagTqIn6FkX8BiB0NYJlsJzXPIbnr%2FILCy387FwGk4gNu%2BzNLO2i%2Bzvb0xY5WYWHsgXI6fooHjtIMic97D7TyVnrsn0Avk5A8uG%2BFsvN0HV0MMUqMUmGsaV9yrbm1BPAKBuBmfFzz2pxFlmrbRPs1Q70e5LIcAp1iizqiCcVp%2BPh3TG9apphoZQS45VJOAKBXng6HaiXMDb7UiHtsRCB6yRhFZQ5r4zKNa0F6%2FChcZErX%2FpjsNDDSxHE0xFf43z%2FDrr8K%2FB%2FRtVDmUym2gVgyomAQv0acvL3mfQu5KOYgdgUXs2G2QHZ1snDUM9DvYrZkWiJ%2BU8bjX5Kdn5qb%2BLvQ41WZdnHT2MElxALXpPTkkrMtp6dWq3E2aMsJ2HEzKoMT5Bz9XC7VvXKGbGibF8%2FpKuYXPOzP5Pemp1x4ypnZaCa4nJbY0JY1nbfVzJ17%2FZP2w6Jpu0fd3hKYQ8EdXC9cISpWBikt%2BUPycZBbozbaHdXmMJ%2BlqccGOqUBiNW%2FtuqVdzISyjgXzG%2BYMWsvi%2Bn8t9EYn28Gl29vM41y9w9gOMUpBZO4c0HUhhP4ZHrMLGF0uO%2FqVVU8oq8Q654KzLDbA%2BTm59ng7d%2FWZS4wdjEVbQ3gHnJwZCTIvUIvchV0yvQ4x21DW74tsSBryO6i3fcFgXewZX5eGDInFMCtjTQdBaxkiKTndRaUfUslSZLVs30hU%2Fo1Fqeh7shEJYpaxmrH&X-Amz-Signature=5edb5ab9b26789ea6f3a5344706320e7c56619ceab1ae11b833371f7f6a5e396&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

