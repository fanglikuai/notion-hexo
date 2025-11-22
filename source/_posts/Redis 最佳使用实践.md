---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VSIV3DU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJIMEYCIQDOS%2Fla9JsW%2BWxoArncpraXXQvmD0tFfq%2BcOdGzRC2XZAIhAOEHEdFfVq%2BKOdz%2BDW2FQDhz3ZdYaltBa01Ckq4ptdqCKv8DCBsQABoMNjM3NDIzMTgzODA1IgzQ8je%2FvC3aFuIyLIQq3AMoUlKQDuXIoDnbeMFd49jG6gUahMhmM6%2BVW5T0Szh0h6%2FCAoJMMHPs2EZ%2BCNdtscA%2F3WKQRT9sJwaNUyWWLsypVRZCvJO8bMNjJ1Ct6PQEby3NOh3sLdWsuWfr4DXjXPNKzhXXzzPHrPrendAhnt8cdp7DALR%2FngCkTzQzay%2FUbk%2Fphfe8ensdHCMjr5Cwzxtmszk4nzBAjxwBuKCoY0Eo3L%2F5sYb8afuahFvY9akR4AbewfbQbX6LSpqwW2pcfcZ5vgV1CJP6q03jQeBWx%2Bhw63Nd5PTMIYsF3b6q5TvjxbVSlqERQZ6Qma4rw30FjGpxcnZoG%2B9IAqplMAr5JTKm5%2Faoqm62Sc4ISvGD9Zu7C09%2F9la2eGqshXH3Gh5nAVstnGGu4TGQE2bqJ9jfrVevcHV%2BIS8tLi5qMj2lsR6tIh3ExOpvKnpOuPFWtxsNP2kq7gJLc070LzNwWFge%2FCFggpxGpONCpzm4cl2U3S13lSxuuPlO6tJKcEUWqtwtuhbolM5xTt3vyTuNWlvgJMFZgpRvcr1vX5FR0Ud6tz5zMv18tiDZ4Dlomu%2BfDeplWhqEYQzfxs%2F1QQckJdXPY5c5W2j4aBnY8FDfTS0LUcCPCF%2Fo40aK3pjQlmvsJzCQq4TJBjqkAeN1tMZVJllBfgpcFF%2B3404hzNv%2FGAM1m%2BntNpMOecDLgD4fz4gzebi8xwojj1zpjcdjbaCpeWqBaAxnj9Fs0hEXS27fG3L57t2%2BDzI9eER3jISahCxHHDGQCs9W3F%2FtY0TVykIl%2FIGHp6nyHu%2BnaullK9HsNEaT9VcHiDxn3dHtpX260TSHwT5IrEcb0cuwl4%2B80UCQg93Xoou4mHFIBiCl7ALn&X-Amz-Signature=213b533ec349c1597cc31ba51e718da1a7ffdb7350c13e104492d3e7d0ea4ca5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

