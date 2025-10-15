---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627ZQ3WJV%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBy75OJzvjmYHL%2FHrC3SgzRUGsyF1NQB7iyRI%2BIeXQyFAiEAvA3Lxl6Fc5AFz5x0IprQa%2F8YrqDTR2AffqRNddflcvUq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDIdT89ixBqY0%2Bubk7ircAz%2BnwmLAzDlkI4F9GJymiA1F9TJpsvRMUJs0leVv85m5zGnTwUbF%2BClavVHClVv5vhcC5OY6MPFPeXyhWQJQXrIBU36swsmRx0SAz2Kfw6KVewLA3fFMvM4msX9HGHicrFc1Oxcb0ZWlwFgQuU7M%2BZs5wH2uXEQ74Ah3fX6mJQ679aGLG3AGJRuQqyVN4exh7k%2Fy%2BSPnzenTaJc6BrC5opmy%2FsAqoT1yBxkAwYnwi6NgsYnODTJqsYX%2BtZ7JB8F4BQtw5qFEc8EdG9z%2BAoz3GwAhWLuTm%2FEIhgcfsfWGQ1Ttq79S120OlFkF9VEIBAiBI6h1sizwEQA%2FN8uZknAQnKl6xQPN2tzZvwPLJdo1JhTQxkCtW%2FgRl94%2BDweuoAECNeOfr5pb%2Fr2Joqnzly%2FIetImYvrpmveSWI%2FWv50LccCXw0xZV704gUvlMFtl7Bbek1nSuU%2F%2B67Bx3ZvhWtVcHOqrWyd58ZmX921whsEKifv58woh9Gb9VARe0FGH%2FRHG3IR10NqP%2B%2BMXU8UcTE0rqciblD6WwqFaxvWrr3gCbxV2U9PbgeS8bEjLTRl%2FD2FW7iFNmmJrlFQb%2B7xi%2B8LjcQsuPfOZQK4lF%2BYj0l1rBQReRXxwTdqVppgwiYtgMISFv8cGOqUB%2BiOWNho7VgZWKlAjTXrf9Fe75lC9Te6cn2Lvo8GIvJoDaevptoqn1C%2F%2Fw%2FvG5YNJdrVdhKvPD9gyP%2BJBz%2B6ztiBii%2BxMTy8mVY5q3nITbwkEs29tsEcIa0NbzF%2FebqVjCZVh8dmfdSZQ3CYGOGVDNozImavkHhIDTLUfRtNWtxO19sB0TFo0y8ZF6c2PeaCfgJ9u8p7qFXosRsVqEs%2BDTmIAlT%2F7&X-Amz-Signature=a1f944a40da43736f2cf8aedd4122053a0936289d72b3d9fec6fae62afed8a44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

