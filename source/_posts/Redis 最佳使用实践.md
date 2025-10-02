---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBN7TV5G%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAi89HZTGGbnW8NQ3AkjQmqu9mLM969VXtBtQNVOIjLdAiEArkGu6ov6WBSkGwp89lz57IfkVUlAPcavvmz2u2D3iW4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDL69kUpvf6yima1bnSrcA%2BjEY9tqlE3JKlaWZhWBeG3%2FO7zSOUJnYYLKhboh3Q2rwMf%2FQN2Hols1Zt2ZGLhTEmXFNoRW84YntUIOsbmYLFC2feD84c%2FgqYDko2zz3iV6db%2FzMNFA1Fq%2BJMJkgM0naTsRDUjMP4VKG0UryBbldxlBf7Ba4uRcgYFMxlWkFUnrPfqo7OwMYT4IZ4raEBWVza%2Fm5u1ZxMIVbpoR%2B80TkHrg1K5NRP2YgMcLZg4lprCrQDPWvfW79LIjaASV66YaQBRHlGtMnp7uUyAJ6q1K5F%2B3Rj7yrY56AaFfsnBGRWbJiswZIk3AKdRWjic5lze7rxnjQNeVkqbbadTlA%2FtffwZn5G68%2BPWKEzKUgZ8wbGW2FeR%2BE3Mthfx2MbsbRFb%2B93QCT2EyrPCkpsoaHjPdIsiMEjkvIX%2Fw5UJesW4Bxb1PBLCweQ2caep7Ql2HdBGlVPl450Kx9CROCvqbq9K2zQgrMZDsjDOZ%2BINNPdejywLoU4Rea7rs1j5EbxrcwDL9DRwlKcS2ZkXTW7CGXxeBNAPp1uQwsUc9KRD02i%2FWurdRG9K6knuluNR9%2B4Jk4OD8SP6g0LBbX4kSxA1q1%2BkWJLGzUV3%2F4Z%2BIL9HKwYcQGKnN9OERCupWOs1nJW27MPva%2B8YGOqUBD7KaDKZXcp%2ByKhqTMLlL%2BRAKRTohcIcvtSYJG172g3agclBHUvo8ZrhU8CNRzkJ7y1pd%2FAH0%2FMUVzdRoIjebkrk3coeir6AjScrRGCr8AyTyaDF2w8seT%2BjGCi%2FAqW8p3hFK6JaC0dukSPXTmxGspSu9OT6mqvlN%2Bmnk8A9zVxk2%2F7v%2FZ5RfLC5NzfnXkJz6ZoibQjv5tMmUBCyWTBSE7Idy8nxT&X-Amz-Signature=f86cfd48ee36c9795e8c9bab13ee1c50fd634c56a12ecfe87fb767cd38029ffe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

