---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2MBRSHO%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCb4OIVOVIceHBCqbxzfpy%2BslRaZ9g3JIeBr%2BHN3sOmwwIgUS1XODNu3%2BLMluO8bXJh6%2BvebF3zIGhB1CmkzrtKdpwq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDPD2A4tIPaTxfu%2BNcCrcA2f5%2BVHWJaaXVCehWiP%2BAlAQ%2FuCUfrXrltKSA%2BcXSiOdZ9t15eHjQTZn9E%2BZh%2FdMxqfSuL%2FFTk9DhQMnfNBXlwPcn0DEwev26b8IyJVuYQBH1LYiU2rKFMJrkBJ8BTi4Miz1Hm6ihu%2FdYAwHNww%2BqEaT9tNb39b1m5cEan79FnodD%2BTXZMlLU%2BmlHkQJ%2FA2FWZOD7HoE7BVAGCqO2bPq%2B5EK7CxGRd8ODj1WJnq5iOfKN61N8kJxm793PvY8f7mvuLm7L1isvUPB4jXqHZzRgxlfeZvu9eG%2F2lqPW3djp7XuOTbxMFLq3Pg1LN6XcmEssF8mgk3J%2FTJhCTcvdtX5EU2E9JCE9oXdxlbAw3KXLwAdfrqBvuPfseDE4c%2FeJeMX56jyVHdbqgtG1ZjhjchekJp9GYQRQ%2BzRIV0w0X7g4h4zk58aqNrtLXZJNxnyxJHD6gmchfAX%2BYh1W2la9l%2FKcT%2F6s9%2BPZLkn6naVC0EYRv8blHhwvTOpMINSo5FEFrLYC%2FxuzpsFxg23F53gV80abVaqc9jnB%2BRwpkuFVHO3591v1AhEhyA5XE5sPJdsTwxdrhitD%2Bk4JrJXbHGDC%2BB04Gt4qJctWWvH4R9GOe5l96L8SMX%2FFej%2F54zuW5GFMPT%2F5scGOqUBrlrrATuM2Ne%2BnNE1Uz38JfafUa36pwjSuoBoLabjFuXPQObJApJrOfLIJ8ThSaxbtEPvop4iMu47aqKnvSQnNQEGRgTN9QkW13wEGQKLbgeI4sGnJBAC9275%2FJYG2ZrqDdeZ9jB6OmcIQ65v0TpJ5xEIPMypA%2BXPJDVybn7tp65DH1u2ECCg%2FoXwxCDq%2BhaYmZDhCjK4eyvFLpMS47jnmZOP%2FFpZ&X-Amz-Signature=5206055743da38eb81bc02f64c6c0793bb7706060c9b88fdacf8301338118486&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

