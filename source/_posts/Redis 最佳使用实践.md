---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLB5WPXX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWfzqVUO66VOOeXKaqvl%2FgN%2BpQceilbZyFHRbRG3IeKwIhAN22BomkSBze%2BSVQJGNyQ%2FefCW1Nxhho3V6mAqvoCx6MKv8DCHkQABoMNjM3NDIzMTgzODA1Igy9MVx7jDATHYsfquYq3AM0tdQn2xmgsNieSwHcT8SJyf1uaQCzm1iRmD0vok0mw%2FM6aUVsxKVw1RPpvYzFB8uuAdB4Klett8xvhNyqqihWlkna8dz3DpW60%2B7NvDFubC6fUZIMcCoC6xqjs2bmr6C2JhxPy9nXt3K4JbU6tuy14ufW0mq7Z9whCzNQ1XKdsm6LjkUsqFdW93r6rk3KEurFyuE8V6%2FMDYosZEwoBIGMCScWcxCodfaeBZHGjp4o5g%2BTXUuW9Acq%2BoKSCe7qtChH9hn2HtLOh0NPakFDCt6njQeP1ecdA4TZIvXnYjLJ%2BpCm58GhVfzroWb4ucv7yi9a%2FRVqMWv07YdEnugEO6dTgAQMxjoVDFtsaCZp6IUo2PzK8pKr%2F%2BMXslJLZwOhAkluHkyF1C02pnF5JDOCP%2Fktr1YrnEP4vg8tgwOUskGcRSDy0o8%2B52Mjuz3qR7JNyCiNw7LbmoV1w8jiUstxJkPU0F3f%2FCzrRhhz9FECamWHVTo1lFI4wmen4vmys%2BRZ7OmOpjRjqqqKeqbDp%2Frt2nVfLj7DCpSi1vJfWvcdEuINjVuGSKX1ACAI5KVufOMMNhZcEloSKnKni63dGrlYRCJFcpc%2FhSRnmylp4f00utCqIH0E47WOrqNTHQMB0jCt8vPHBjqkARzTSOa2qQMzPFWL0WQoqAvIVR%2F0vsTo473glfoOopA0h6fIerT5kWfAy%2BjVuS30B2aVJ4QC49i8UAL6uQtNwI6bRwAWpOaXFQkbSV2zsJXzI0p5WnL%2FUgHC23vTPGml1os977mtEb%2FYuFC8CP73LPCy8f%2Bm995YdNAUlji%2Fr1pJIx9pAt%2Fq61ZIci7dJkorTxZsZzRcfPLFmVWpcFw%2BnOPKmcTk&X-Amz-Signature=45d83ab98b3c9b608498aab3bf63f77c8d704c1490ba8a64cab6feec616d7682&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

