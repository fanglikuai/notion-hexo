---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY4AGNCH%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T150057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDM0cKgagPlwgdD41DUAhn7Xv%2FyCWKRinlNR1dmW3yyqgIhAM5EdB%2BiOiv8U5I7cyDIkQqKzwG07DA%2BBUWRjVujis8cKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBVKpvQmScEc5GLdoq3AMukrcLy%2Ff%2BZNHJt9EX9Wgml3T7y6jKNFJUmM9LZgBZ4mvsO6bYKOlsSuy9r%2Bg8Y4ZIdaO6oQjFWsLEbcQzVijqmLgApZXoMokyG%2F2L%2B%2BaeKKLPyQvQGgXplyB2Lembe%2BgkQu4WABmWCsGiPG9%2BGZ%2BaOQ%2BhZb03RLYRRDB79vxlnHTIhgsY06GN8D648%2FEUl1t23ownLaQTHO1iHHhecy436dCHTRwc3zFX6ieIy7WL4czMJKYjALBZ45mNVW9jMshL6ksjULi0aUWIFdxfTxlyGr%2BLDkKGNcsvafu6SmZnfVQwZxR8p8xB43xmaLBlMw9HpHbD0Wm3%2FjAmop1EaTPuZe329O%2BA52BjCTctwferj647%2FJ%2FfzfPoMaxE4RggN2PWRfpUkPXH1PHM3OLYVCc494eUkZni5F%2F7OiuR5As1kqKkfJPvciFXu7LgyVfB72PZx96Hd9LSI02rCKW3K%2FvG4uUHve4nNz6wFX9tOHz3dwHXiaYkfxz38E2ua5bSEmy7XfSqXseUuY4yC1p2K9o0rHg7p6iEcUXZ0IoecDGqWtY1c7ffrjfyV2cwClA1jx1rN8aadzHJL9pt8uEg1wmVBT4nIcZLrxZ6oW8kMAmYjar8bkNeyM3%2FkqL02zDQis7HBjqkAW3fwES0DmHqmqk90fQu5SkcXfHIAF%2B4dg9Tno5WDIlnq7PURullB1V%2FOp7UFuupejd4xdNFAjRqkAU66vzjnhDKCglRZpNShm2SqAFPoKV23bWbPzn4Yh5Ped0dXFVfZi71sLWDV7YR9GFseemyJ2pN1w9VcAsTniTX1FR%2BZ5QP5SucwUOpeYkyDUy3hhpf6Pz1mEussvmQ7EaFs%2BY7iVLxlqs6&X-Amz-Signature=96fd90318138197f7f9590744c16c088bb086eac9096a7e26964e5f072b054ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

