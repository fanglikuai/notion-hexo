---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46654H6Q6GE%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFO8MtAngPlfiyGRsKEkMoh1snkVBrK1J95LP0l3DN06AiBltyqVfLrK80kdabmq5gJJ9iVDw8qj80Ca293P4JUurCr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMniX1S4MXzxLai72HKtwDfho79FP07Kf%2BZrlNAJjYz35hBQvOJoT2%2FjShQq7DQ6%2F5U4yn1KI8Hh6EWVlL3DwSvahAuNGPu4%2BW0dAJWGmofQ0fU8vtxQTtvECsu%2BiQAXfUFsK6B5Iy3fBnEUBeDh%2FMYgt2P98nP%2BqHffPyjCwi%2FL0e%2B4iaUddum4yzU3pAA4AL3IetaNoOZ4LOAnDhJEwW%2FP8zyw8uyxG%2B7H7XQ2nL2mThr8QPNT9lSUw4QtiTqPV7yOU7c7VaE3daVsLGoCdr57xzXzLBCxlYdLkj0czUmp1EldSNII4KyogVqVebbaOQ9NpQOtAUR%2FEmDZFaOFIJNwWHly4DhXCM98zEby0LupQZ%2F5DIG4FXJi%2BkTlu36qsk0RCEzRtElG6ML7GiQ4JJgGNrE%2FBLbiCPbcMTcnCiecY2ebX3jTqZrPOYXtEklH0yX5mzHPgtFjh%2BnUjzg6t2rFL0TDrIvvx8kBjr46CLtu%2FXclpdf%2BOQ24kg9VxW%2B7D5EC1iQUu5oABrhgOgpYg1FvoN3iLxsUKetrjrYgA4Pb3twy5Zi3bDiBFy7l5z%2BSSBw6LwnZN%2BET07tBXlqPDPc%2BbWoAj56y2PyvNIy8qwBxyUkrW5q%2BOp4a5cQy6B0AGwUA66%2FicKA00juRow7quPyQY6pgGyJaBt6F8PleaoBBfJ%2FpfCm9%2B8kvmiwEbiYq%2BksTD3d4jmEx%2F9FhTDByXKKfgoORDJE1xuYeNccMsZGMLQD4MvHMfa9wC%2FievmQlMnCQfHdEhKYTWYKBcw2Y71yNqmsoHinj19porfaJ%2BbkDMqx10xk8Dg5zxJvAIrC0I%2BIdn9U3kR%2BhRw886YCXulIvoNK33synd6CH6nGGqiejMxvw4Z5FPDwlH7&X-Amz-Signature=e5c23b8e52abbb78551b114a69697d265877dec4d5c608b6b15b7108286f97c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

