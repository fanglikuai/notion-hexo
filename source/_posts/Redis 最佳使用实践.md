---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFGPTL3Q%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDALyi0ZukRv3SRB%2FXyygHge9bzSHC9x25zGrMjKJJlGAiB4Rmk1aQk8yPPjNE3CVdCbh2oSv3QP4H2wxB5AgNL7iyqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZySmN5hkGqKUrobvKtwDKtbQ3OsGfDWMJjoMunRwqwNR8LGrcMnAnNroIpQQ%2FMjNV9bXqOk7uk9HA0KOw7B0BVy5MXp4KQq9U0V48UcclWRD%2Bh%2BvHxn3FQUrlFPCy377TNj2mLlgIpFq6EoPiHXPeADX5lt%2BvCpPF3AVzCsyi2xFCKZrrSQQS9lK9VPjYoGoDy2AEi66rN7prQASFhvlZja%2FrjvXIQ%2FrpogFMpBpHS6ChR2AtW1DTmtQWm5KBso6DrWTmQie2R7ncI2zZS%2Bfrv7yj8EmApcM2gE6JiiO0x3eyqTL9sAMctw950gGbS4JP%2FQtReKKnJuIcLGTs%2BJ9ZjESVp88AAu%2FJxKGzO3rmpte9wLhYAWw%2FPdQTIwzQQ6WjFDllOR3aakrr5jVTp0Qa%2F5eeYkIGFT%2B0ce2oLuNCssOW1sSTdBbIkfJCjivMADLd6Wh6YbdbArwHqp7RVAjK9N9gjYbqV1m5MzhhxuLs98aq9t%2BryLVFL4%2BTH3NiD%2FVMLjP6yuJObM3cxC3b5A1BzZqt7S09tiwU5I%2Bb7vsm5I477mBrZliuq6Cfebm9RHuwOzEDqMSv5czDbaqLlZaJwpC1waXalHZB%2FnQ6AYIaRskk3J63%2FYzgf050z3tgeXl5g59aRplepUGmkAw9dqtyAY6pgH9qWLrLffTj%2FsbfkuPNdU6dULq2Y0RaCN1qTLh6J26ktIVgtlSWM4wPp%2FEq4MMb3gYWhwPzPOtm5wYy%2BAIZfqFMPCu5oaqfMVTMQNTk2aSJnlj4eyST6g2dKgZaPfOd11iI6okuekejk5q95btKTpO1CSOOi%2B198jAmIqeuz5cLs4hMyykkvU%2BKFmcSD00K0J%2BZaHUlCQ%2Fiv0MeyediYWKEFuvQXJK&X-Amz-Signature=c818157fa373776295471506e45ebc067a7d3d1e9af76cc6c040d2ef40e39d19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

