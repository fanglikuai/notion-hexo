---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XSI65AOO%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3zoBsk%2B1dOjnog%2Fa%2FH7Hdc6%2BJf%2FAEhtXM7xrKsIf%2FjAIgM%2Blr63mI387oggPq8Kj%2FFPz1xY2mlw0xOd7wQICly7cq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDGeWAsAX2VGfV7v1AyrcA3IlOpIODm7WeuxiXWTM4P8RviQOu57nHHWZb5IlGs0L1ESluYeaOgAuWmW9Pg0zDG8CJrCaM%2B1bxhRiuLzLgz3n7ocizJbqPMAYUBwvuAGS7%2B2HVC3fsO%2FS8QAmK1RIUp279fGbxXVD3rNFsIEWksf1Acx6Kivg2qS6txXOepdGaCvV87yyRiIDDsrjGeir8KOejF9i2uvFkXgsgFgJtGjlFSBwQAqlpbjqvz7aEuQ8yfhHfcNDvcg8Zd8ykctLri1vNrs6gFfWKE0Fl%2BnU9rfR0%2B3s8%2FZd8dhhWyoCjPdEteToZGLA276PrguUCUYeeTSZAJSrpssaJt2arQDXgas1jlJsPb63sb7c9UnAiCHanEXA64k5wgPeDPtqMWP2wemnfAbQSDU9crOe7uiRuPRCM4cPOeAhJ0uJpDa6kievwxJWiXhsJ0L1qxEpk7ANlkN0xnK4%2Fse05bruDwsYs0dx2Z2BslUaYEaXDDijhM4DBDnpOQGorU%2FnmzT4Q%2FYDBrRZjZtv4MAR5z0efQAqyJzgCe6ocTYSViay7SpTJmU2IkfYprg9KcmVtEaSNcctxWTTc8fpXPwVBGY2exkaZ9W0rHzKeiD8SqpECLfEXw%2FfhXX2%2FFYf48f0wCmNMNSel8kGOqUB%2F7YybjekT%2FEXtVAPqoEol3U1SGFzHI4oYJFX00e%2FSCAwHWmf7atyh%2FBwnXDS8vO9H5A6sp%2Bek4fFGoJz9xfASLrtgIgae1gULUA4%2BPLRS6p2KiYwc%2FdXscDfF6iFW5N9nYXUwVMNnwH0i0hMrV9AroZXfzLBgg3bUZj29jm%2FZDE73EF%2BfhN7uAGiJOFNmEIovo2Ph6g9ygOwtxERYtHc%2F7wxng5j&X-Amz-Signature=5e08887f4faa18da314497027b62c0c94abc10032b5d04cdafb297f65859b219&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

