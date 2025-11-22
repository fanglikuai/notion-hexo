---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SO764X5E%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJIMEYCIQDd846ZVj%2B8ZjwEp0MIymXJ5Adn2PLmaFNX6eA7AcuB7AIhAJ6T0zmQtJYSpCb8apgn5Eztcd72Ca2tGiyN40GoqgKHKv8DCC4QABoMNjM3NDIzMTgzODA1Igz42e2YeLPHWTWuugIq3ANASOxOgnteyO4IpgI0ImAmlcv2Sd92LK3bnZm%2FfDRzpbVwzEdqyG9z19ERoYJoyVbRkqsRqQITV%2F445ojtdm4KuCgTJQMJ57%2FKGM0eZLbGBUm33DotZrir820Ca3AuFXcFOSwN0YQ9Ygdhh1JAUonJcrK0xC6DoALn1ZV9UKyShbq2ClNjCpQchQkAp8KTKoXWC3e3nTAOhXqlU60xckS6raFPgIFX4XWlYYvS3GD%2BTWPLEQeKPoum0nzCexBJSUeUa0%2BBtMHaHpNHMcCNsdm%2FmljVZqBENFE1euZWReK0VkNwoNKE%2BiMH4TTyj3z6AL74bfcRH3ZyVwovsl6Y%2BukX%2BhuXSXPzxOPLWvnTI2w0cFvKtbf3c0818roMTnhLWQ5LLwyI4O3%2FMl%2FPfWeD4P8TyUS4E4W%2FCwMNSNN1HDD9NDx9yBh9zcDiwFd0ncmIzVQoGihl9vxYYZs35HisM6ozpRqiuSw0uWd9wQCRaWCJGhExo3yyX9r%2FK%2BV%2F1OK5B3MDvPmcZassgsACyopLu1DioxWKc5lsMcoVMQQGXqOaNWIBYaeyKCtlAEhVRXYg2S3mng94F4VNjsUaQ%2Fim6cMWz%2BSpvwilJLOmRt6DKgLkX31YHBLdUZOGx5qBhjCUxojJBjqkAcG8coDXYO5Zr1E2XKk2bKnyjbNYusN1MdQEulHDPlPkwqJNN40hdaqXqag6PT%2FI6kyLlTkK0PgHBrpiW%2BPHxve1UASqchsbdNN5q2QeSRyA1vcwj2yOo00nBJbmlh%2FMo6PDtFdozblYDHoeHn%2B2c%2BQ%2BT9sYb9ph73UUhy%2FZipm4MbEL99mCYa5LBcTX2nf2MZdnfXmWWTkLwJzCKfLEtPYTNie7&X-Amz-Signature=d9040441b4e020e1c07b066dde437403b1a16af18cd35f51ed2364e1c41b25c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

