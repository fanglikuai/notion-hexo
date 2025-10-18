---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDY4RAPL%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQDMDLtx82ecpGpxyKZv5H2kyG2J%2BhhT%2FpmhcfxC5CABJwIhAN4s%2F3BSlGSJkYywIhlCqsOq5TBPM6fSeLmeiNAEpk4UKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4%2FjzeB%2Bhxw9HHhxwq3AOa7eHl7Y%2Be34%2BuLYZmzWrE601R%2BDzfkjOwwbg%2FkeIb%2BKbQKjwBfcopZKo655B8T9BzDxuXxyICgAxopv0BcWnpreCORcpYeb7Oi%2FXNQ3iBxWQ%2B9ffIDVmxhML8rLUY41KlxfGunE74bqXno8DRDpsiWlktx5TNR4emq3Q4BexUrSW1J2kOogkXSyOo08bBiVBOnp4ZmEl%2FLOqpau8VNPwKCZyGyyqLZurFrE1WFDwwxeED4Osc7vt4oDB%2FVlCiBXAokbDcPGMj185cr%2BuEjrsm%2BvRQKspFTC48iopg%2FygIdWQMzhjSJx7Mqf7lxx50xWWziKd5fUKw4dJgYsFMQVErZBBYRAanEyF4iarMpHvAAftvgAb94QOHULlmr2tHCnlyKe6IWhCuzBB6aANEd3tiXRIv7MzcUrmwEj8w0z4UckHZr3JlzsahmZHLgZnsIRTMllvf48MCJkoaIDyndi8WS2XRGtga7QAcidxkNkVzZbcmhrNSzNPlcf9Gvy3%2BTCQG0dE%2F7YIalICK9BTx8rnbEDlOdiCDEoxIfPldfMmcITqP3kfdDY1%2Bay2tAmGnQ6tQwXHFUXRZ37sz51To%2Bv476QeS2kQpH3xoXFjbA2pXbqBWcCm7J%2Bb5s9ANdDCZyM%2FHBjqkASwXJtH6xllqmtuMCEGoVoWt%2FieHkD6qlIzcNM3nUL%2FMe58CA%2Fli1spxT79YSPoATm2qDRtbEv6sS6JYWJJWTrNUWEiBRW9t4kyoY48R9bFDHyE7UFQcfrmzPjBi93t%2B5%2BZxAf4p17NRPRjEElyH7xIkAIUMAzqKZzAtweE734pCTbeBtp%2BftcHFj%2FoejSDGOaQMNVV3AhX9v%2BF7GTgwJFhvY3jK&X-Amz-Signature=a45bfd4699beb592f6af87c096b1c6d6b96f8e102a4def45955384ace77f0f27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

