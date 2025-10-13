---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WODLBJSX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDr%2BvHVJ6j5GFuHrmLZEJfTlqCz0ZBrXI%2F%2FhG6KMG0bQAiBYSJS5L%2BC5T3qkVnVu0bN1VMtQwVfuAENe3Phf9Xnjlyr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIMj%2FaIE9%2BB8IbWeo%2FvKtwDTlMXmRYH%2B%2BF%2BzU9HMFnzrTaKeBp7cdI4Sj5koXtRF5a1P3mNwbw2hrg%2FjantSGE%2FXC6%2FS936%2BBmG5uX79avafke1z4l0iIyeDrFa4WjPOZMKorymRQnwckCvCI%2FIBM3dexugGr9GH9s20TV9rNUjhDjAaic3viCOc0zw8yXm1Mj9k29SK0utvCSN0sEfdFcb8I87wn7T32WCp8NwmfN4Gf%2BOrj73CUbtr6abXQTHU27UFskBpo0MImjALO4FUCcp7PpZyUAVjYKPR%2FbpDNKOAwvmbNMPe%2FnzmbW4OUwOCfwXQF9D0Hpw0KdUrs%2F36sEgSHiXHerWUvK5KCeHA9UvS6vJSIR0ohFmHzlAY42gPu9elIB5XIFZJ8deuCLuzictZMO%2Btedhtqp57fOmsD9xq1Ov4IM7V6lJVBoG5M6UmrkTMYN54djsBCbzMSG%2BnncIyrzdzT5sQZFKF2aAJBBYPfE68gRYWjKKZXShhe9d51%2F4GGCwHqfJc9r5p%2BKpY2Z7vS8jptahsbyIhyNX7tBgrahJ2SGF6wKgJLi7bzqqa1S3XTMwrbDz5EDLkpElG%2BSTciq4VG8w%2BkcrDYdoW%2B9APmnjT6gBX%2FQv9qYNexSZIDbjLOk1OD5DM1vnnqowpZC1xwY6pgFs0ZIS%2FtCPAnJN6p8I28ofS8KgS%2BmbE1%2FPhFjapCdaKdvnmF0X%2BZJwfWqxefd4lp85LfeBYfGWMf2qEKmMivO4SQUFRnI7kSiFRrC%2B51w9vDDoZKh%2Fa1X4qzYapTpKH3oSH9%2BtBq0%2BazgJFYvHcrhGbvXp%2B24xv2UUBId%2FoD7G9%2Bn6eFJ1lDiDTClzd4wM0%2B%2FvuGHDcURzdETNzHB4M%2FIcmH3LnX8O&X-Amz-Signature=1752485042c601d223d5071fb0693997635a930ebae19789c05335730a604f75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

