---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q6OTXMAJ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICadG%2F608L3KbG18FDEkYam4Mo8k%2Fq8Hbrj1btAj1CCvAiAoU51IlpNaN00y5lZ%2BLDFxif5iUn30Hs8bS7xAwrU1RCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPZ9kV0GCKVAhyOKCKtwD4v9dFCcwS%2FbDVRyQ%2B%2FJKlhl7T3em9khMACSk8gYRvTqpJAXJC245SnFgrXYB%2B8gmIACkyJ6hnOVvuOmoT7jjdMXkTh5dJIO87rC1vPtKpmSqd3tk%2BDjyB6x4FULARvsDGPV5CagRnx4Vsu4qwTqIzscYl08Xd9mm7BtpFIqDiCiMUg%2Ft%2Fcgs52GRe%2FOfEZwqQ2HVGez4SSb7z8Z1jYhJpFZWKV%2BsK8ZbMP0hjYG42Shj3zh5cp%2F5fBDwA0qRK%2FVKfonXccUerG%2BbU48x6ihedGyykEyw4Do010gO0zOUR8u9a%2FlzEhtvoAz%2B2QgttLIG1G3NsBCBpJqwC0ZZ9qSA57UWJbvZBJD1H1lqj9yB4um8um%2B5PHES0inxnpGn%2F3pTcQOuobLq0GzzLRZ3%2FfLUlUz01rXjy9%2F6Wx9ItDojI%2Fm9XwiEckwaHiMwLDYdlkUyIMS43kbrXm5hsEcBiWmzCFgqG%2B%2ByvIwOCMHlt5nnitPrEOy87c%2F9B67QJF5ajMBUXYJsIbRQA7M9%2FaB2MDtQnGOE6sYnFQ35evZtBMOl91V5ptEL9gf50Izo3HAxPmp7RhmpDX9vqNElAfATuMGiX%2FOfsOgT1mRWuqzzQT6rmcsVG3yMpYdxbrPyyF0w58XtyAY6pgF2beD5eI3gOeL10N8ALzC7evqaCi4eNfxiKyjqz6qyELbBCxbnfrUPBK45qKRmees5kj33L%2F2ZSZ4euQSRR86ONANQNtzibm%2BBzRodDShN5SJYu%2FZoBTIkbVmtz2%2BY4BOICBW2q0KCYEud4uPXxN8LS3Jv2e2S3hWBtO8jROorevseP9rtNXu9kl7uSgQ0naJnkR0%2FM5IavYIXBODK9QLj%2FRPOh3TY&X-Amz-Signature=41206dd94132499153931e3edad301b9a2e6cde26b1d644a14b945927de7e86b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

