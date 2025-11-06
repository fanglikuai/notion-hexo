---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TPMUGB5Q%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICtdJfzrT2y061n2jJ%2F1Gop%2FPzNdejZxn0BiezFipig5AiBSWdgqFjJmvV5RSxxyejZxVhvcLai1Y1yWsYU%2F2cbpEyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhNd6B8uVcgECethLKtwDxvLjbRNR6eYgrFl6ktiMqZpR3GzY1KrgWnblEBcqewrC4ujWeNadx53ppwoOgqa0CNWPaxd6PFQUYP0C4QjJaLNzxrrqHUMarF0eeQrstDMqtZWDKUMJrSRxHLlcUgTCfIpLkqjQ9USHN9IgqioRkr5OTbnBO3io8JX4Pt8WUOnuVbzgeWSfSqnZS4rW18mu%2FgddLM4IH3rLJ6WpqeyYNTq0qnforfojHDtcQ%2FDlhzoQ%2FOYo77UQrbEyvQ6Uszkq%2BJMKYhL66qWHdO7mr%2F24Pn9%2Bbr7d6nQvQCeWQLKguqq9MT48AMm4DWUfjVj9TWvf0ohEbD57OH38qqrAx3KGhOjQ15JZR8kl0JfM%2Fp4gN5zPqbPqhDXr6yx1UiVfnwXaiOtmf%2FYR6XgMNdFhvpBXj2PsSSzAx4WoiLGiK24pXZdIlzl3%2Fi1zeInXsOXBjV1Q59oWY2N8o%2FhKqYAdHC8p%2BWy9E3WYDHMOWPtZsSDe4gMxgELs5Wv7Oha2Xx9YXmwkA7dBoLn0wRD7vf1Byx8KoeYDaykrnHsbDGY%2FTI79X2uedOItryyUNTYZ7h7FrhVzPFP3uIvlIJEHw1tVhgrl8UXmbISMxtN7EFES3AixK7i3h2Qm4rnq%2FfrFdjEw%2BqOyyAY6pgEdHKPk80cq%2BdGkTXJcW5u46SA0gRv4lAJcY8VLD8ECh7ylWfZgw1Ip22Zrh3jkMciOMBY44CwTn4kbkbKjZTICr6tZW04j%2FKtP2Y1VLz6nnp4TJOJ2FAe4EB89yAuN4vkPBpqcTX77jKEYT5sAj9Ia8SLnviw%2BmnTkOdLBo%2BcBCSclbTFT6YaY1fpjd3g2SQQdcIIPx8Zt3%2FDYRSkcrJdKL6VWro6q&X-Amz-Signature=bf34e224ebe3eeb21077c05db3b8c6066d1b7abc49e9b0f810f1a706ea5055d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

