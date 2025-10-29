---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DP3UDH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHv1SehUpn%2FcGskqQbyKHqK1pELvH3lCzgBijIoKA4qSAiA4OBfhcoZewfOe2r2Fi%2Bc1pwG4sliiV8dM5h9yIleilSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXxhlgCO6kKJO0tx5KtwDom1TPOhXQarc4b1hRMlyrIjgAcAmg5a%2BqF8FI2eCO9Tz%2FR%2FqlTof4CTda75J8uq%2Bu58Z60lRcPyM5k6p3jMhCWswf38XV3vNvlH%2Bx%2BqfqNwfBVPbkJ7fhW2iwawCM0Hvi4QuOrFTbPsq4scEoROkYLTNLDbUyc2mQoR%2BTHNeQxL519D1bYO6LbB20hAS27x0Cf8BBLYsAZ4ViXa%2FITX4r0gGmu6pSiVc06rx34bQe9PAmac5ks2zDAw%2FhkqsVBcpn9VKmToJgRrodPQqT6cQJyHRv66hoKKRQb6I8JB%2BpNCO14I57VnA%2FeOpWevyQfuPvTPOX3tQjH9egG64QjjoVIvc1MQDCb2yLTVgK2L%2B%2FU3b3hoXabKA8lpDd0pVuWMI%2FLGQx38IiK%2BeC5gl7gb7b48Lxp0DQPGiM5dQeOsAySGKa5dq6Zdkmk6lP9DylJCvjrUOj%2FMANp50UldxWrIBqoZhlVB%2FmrEBScPwemufDQpG7ZdLLzFUjTDFjFGanJXgXLHmKE4ZWlF3ZdKdT1Vw8e9x40fwfcIyqhZpYgLenJFlxid4krMrZivrzRbtABaItel%2B0Ri%2F7CGNB%2F0Cf%2B8hEMuOK48tbtoJHsErVQhHZHrm6OtxZn9QUnAUwwsw5JuJyAY6pgFyaOf2H9dHhrAdcZaX%2FA6YfjHkYtCcd2ZmTJ8KoE4IYxy1mtPlfnEWT1A5HlNTK5qlEGJvnW7%2F93GvcLIz59CMlk7Z7AB9xgZH9qVnvF9Ea55lRtvyVleOc%2Fm3yP5tGttUCK%2BP4M1B0MII5Vp3HQQirc5wVhAwGJgmC5EjhUtIz%2F9GbpJ5WB%2BRpa4RHntOrlOB3ikQQQgxXztPMeMYbXBnh68a4Esr&X-Amz-Signature=c15d5c5ca7afd44084c2e833972b7c8e3b343fc52f7af38bdbcedd59b6e1f211&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

