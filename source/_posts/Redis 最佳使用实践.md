---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YA6VRMWJ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClWOATPUdDaAFqmibBPQb9cVlf%2FhN4R9AKVOUTGd261gIhALIhalhGtH%2FbrisbjthrE6SWbtL%2FXWoRLo1SC4dD1qX3KogECMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxuu4V4joSwCS1WPXsq3APD1wQIeHeIXjtUXkBJGbjl2AJ4mEMrplOSHhA26JBP4xEY8iNOhAOHk7ydEhBfJwttUkHpjq9grtQi5SibGhzdOckjLD%2Bdir3Jv2t1ZJmfXiIIqW%2F1u7%2BeqVeaKlw87PoKszzODp%2BVSqOxb4Q%2F3T0NHgYCwz5Mu7g79o3Ja7qdvsEU1oVTKz6ZkqYk2nOtKFew26eom%2FFKPuEEYicDYcejRacbgj4PnIwr0r6BFZc3ciUYTFIrE53jiCNvY78z9dGNK%2FEzuDWKZkrM%2FATpRaiq8y0%2BVcZy92Wte16O0TRqQsyM73a2KaZInb203xvRaBDplHNFZzW%2BIgjgs5PMOLbiS%2Bn2tKJcH2zLGiu3pg2zRVkFIi%2BmQowQageGrBMeIO2oV9RHtMs1jzOiUT%2ByS%2BaambgCIqnMWAmM9Y%2FYXZl4IIvhVSlGUsTcHyrpzDS4gzh36LcPc3u6CZKt3ixZEJEMG0J9Ud7du2Jv%2B1Y5IBcyrz%2BqXERy5RcJ3T%2FhRvFzl9JePpu8RTxpsdp8%2B1Bgz%2FuOfu%2F9UvTMRm%2B3Uk9WYiGMTPVFWJwMeO4gPdeiqDjqQgCNODfk0gue3otQlwEu2PfVws8nf%2BFbVNzEo1fEUsvzMrsYKWarIl08v46w1jC8wLnIBjqkAR2dSxIMoWg7Z%2F%2FpgiJpW1xRdFEjlKNwV54C4B43VnhY4zXIurQl%2FOvJoI1ZXIkJyV2pm3nQ1KeOwfdvSs5u8X9rwdaZl2tO0toRqx%2B5IGlPVtfSgx617UN9Ze48apgwQ3MBc0oaR%2B68iLoaeh6X6cdK8TvzkshuHGS8%2FVh4eahN4rUvDE3fBfwYngn7hJH4uzo2oFr9dR%2BCPbYGSzntrh8u43ad&X-Amz-Signature=b73e10a17b8cf9ebbbc8e435714db4c58559cb007f235a83071ceb9a9cf825e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

