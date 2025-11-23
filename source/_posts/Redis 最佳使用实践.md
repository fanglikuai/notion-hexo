---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YSYGZRFO%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCCaSlLG7Vo8VEOaKfSRmuk5MxvW1oHPni4iLV2H7He%2BgIhALosaYuhnmkpdmeoEnmWVNHARDS32w3cnkC66IDSEaGGKv8DCEAQABoMNjM3NDIzMTgzODA1Igx5j1P4GS%2FGgoQsW48q3ANrzgIojJx4Coy77%2BvSGFZFjjKN25tzqaTET6NbXaMgNee2YjQxWwgMAK1h9K70s5Tc%2F%2BySVTZDGAD%2Fnoa%2BAfdluDJ7cJIQJRJnhRVpxuYWftJY1kojp%2FPzB%2BcjvSh8xCrQ8Hz%2Bvh84%2Bzmnq241pMGZ7fAeXVLFHfuWvGZFsJDu0r3BrPadW0ZJSPMCVSdiVW0175ywORcW8zf5yDSLDVHHzuT46cWDpOAPYv2NJ5n8uTLS71eH7IEcLaO06WeFXF3DdUc3jCii2dVh3J6c9Eu%2B67vG8mvfN%2BpGw%2Bv9StZ9%2Fyroag05TVpuUNXuvC41SZ5%2BUFb9M3U3W9BAiZ0oVptvyhITOFsKWO%2BIbR1eT0NSgwR%2FeWbwkXRSQ649le6YdEL6mkUIvwik1Eh4Oaz0nDWrSfvmL9OcA4Ek2ai7uREdHh73krH16GZowLtoAiOkD5S5xirQ4LI4Ir1FAArDFMBGwPqs9dUKosU4oWTB5uxd6wlo8Km3RhNipKM%2Fi74lAHfickIu9wcvoOs8c6k7DQHtffXI08J4f7EV2vVVbQQ9X7dmRYYKZbbH1Rmv322TVzsga5hXkFwhrVQ9ZzL9GNh2tZM2Bpg%2BXxMlZnaLdD6Difl5ChlFPhaPCAG%2BazD8uozJBjqkAdtN2snkw2nhn8EXZAy%2Fs4rzWB9XtucGAn%2BsCmedgLvALZmXxukbwYkmSd6CpafJboLKLaQsRpIwOqPV3htDu1CiRd0RR%2BZhrQCdffsVFZELYljfBWjPjyUMY2Ay84RX36GORhqQde8GYSRVJtM9H%2BgdDrhiC5A6Kr95RERN4EK%2FHZINGJNMgjItZZBpMe%2F89vGCsHXycIdARgsaRwaUUH0uQjJw&X-Amz-Signature=f18704858d6c648960cba38dc640bc2edfc98aa44e4c2ea0cd97fa5f10cde7d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

