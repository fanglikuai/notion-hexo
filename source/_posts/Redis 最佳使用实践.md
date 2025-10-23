---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B7JXDTF%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T100054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3%2FgEE%2Bvbqrm9b47DblIxD6n0Wd3BqTtqAgnxgJ%2BSnqgIgQ6LCxdR3Vi5eZyCMTzn7qnFbT9v4yrLGvFJ8O9Y%2FpQ4q%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDJQAYtk51HuRQisDGSrcA5Z0jHORu1%2BZO4s3EMdObFEapr0UwACyvfhgC2wiiVFnv5K2ciUisodf%2FkABJ8qEW2Big6wT1vAcHvI8LMMKCG8dfMtpO1qvHQlrFFjjPkbaQ9GpZEm7B3MuqxPmbRLiimzSVwTODPKjuORPwN%2BkbrC4RcJV44FaN10UecL%2FA32fEEM9NMMwx3jztMmZyj5HO3ZNl%2FWfIfdkyDdUIROU8P%2FgkXJrozK%2FVRF1UtMRjolFUv%2F%2FYHSCUC7szA1ErKdlIQiVE%2Bd8PlpRc0IOPqgV030md%2BpR5y0s18yFr9BorVKIo59VHwt%2BsxitQK7DNXM9CmPYisivzXNrimXa3WHpbg5z7SN1ZQoyxYMmfZaONbQ3nmvgM0Vb%2BQyMkzBWmano4pGAd1Jkqel5uPgvDYlS6JaOz7T40BHoKS2yEcucevdLST%2BS9RWKbDF%2FWhedMvrNy9OGgEW2TowXK9qjYB7y559DbkFxIBBkfvYtl2fehz8YI%2Fm2TUxe78%2FBuXhznlJ%2BwBU0xOMvJZPjCr6lr90kdE26XUmFgV%2B%2Be%2BfCu5t3jPbY%2Bal3k0rAyKxJZGjSke5Kq9LbSO4BGkx7vRIJB7TFqooHfqFn68Du1QGYOWONfPyzciJZcEmVGaexDbZBMMb258cGOqUB8XCzDnqRTKyD5Yn%2F0glDuLW4SQlQhR1Qz4cFP5pVhl5uBagUrvtSCZBiCJMppUBxHxeAwCSz%2BZPr91FGPt%2Fv6H6gJemP1lhAI1brb%2B3xJBiGh%2FCYRjLFipO4Jr3MMU%2FGtL2wNvqoWiNbVn9L25PTfmgCGvAL%2BE%2FhaIvIcMsFLCf0TSYN3U4tpTPybh4w9kprLySjTNx1YikxnXLnWJyEZMVxIf6N&X-Amz-Signature=72339df3bd25e8f98d668f4e4c5be03f23fbd3a68c4bb722e3225081a04c5158&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

