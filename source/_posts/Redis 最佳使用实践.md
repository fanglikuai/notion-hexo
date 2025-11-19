---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QDLIUU2%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCIQCjlvxH9dVl%2F1VFFdzguSV1s7vMTJyeghsdnYWC1XaaGQIgELaIdV3lPapDJbEtrOqMkuLnmNGKN5K5dFLwG6Vghw0qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIkGeo4NzfINjkLDfyrcAx6DBk%2B%2FMgSnYja%2Bs181GL1fLre9OnHTRNrmjFAp9eQsk3TYG6Eqq9ASr5Y%2FWsFO5VGkYSZrIbxzqgdDhp%2FM%2BFCeKQZoHX593PhWubQkJay4AU7M1WVL6Oo6OQyoL%2BBwpTMQ%2Fu3yJnxg9Xa7A0295HZCPzGshyuUC1ErSVQjZVlF3tNmlDrQ1ILE6NCE0OryiPxVKJkh1RYx%2BTt59tN3JPdJTDPrUKaPueIfIU1MuxFMLNYVXsxydupSQvA4g%2Bh2%2F%2F7tzPkryKa8b0GU5kVWamXyYovMyHDfITyb%2FWy68G0lS4a4oUWIpl0oUqBaNmJdC2yqBxBAGw8X%2Fi31f7c%2BxZAaj59w4SSpwe7DChbZuGp5aO3vnpFL7jXN3YI4reKvXHp8c6ccr44S8FIwTy7Y8SCy1BkCQWBaLM2XrESvwTH7sOnVqlFgTnRLt4pi7zBZBgbsuyBLvjIT0zeFd6PUhY7iu0FvPbnA7hukfJaUs0a8TiM047181Z482hUfOZyeoxPZd%2BFglGRqwRe07ge33QNdbh0EyOM1vvD08fi8x3aPzqcOKcYniXbNhxnXsTVjbdayxJSBwq35vRlZR5w1y3yIEnDcWv581N5o5ZsdLKV8RUeWGOBrjGZSHY6HMOT788gGOqUBeEsqPpe%2BPPuSBK%2FhEszQjAgsyrtwDRkVGCeizFfvcHcB3W3REO5U1PJmu5aBRFCbYQ985P%2FaxNbtFT6hY2QNc2FPVeAFBI9n2%2BGnIr12JhwPdS8SILqow%2BlzMe8olqRwe1hH5uovph2iU%2Fe4EO3BQgi0FDyI7eTtzT%2FloqURXDLEValZ00u97VAY7crjnqK5MRyFAcTJ%2FvuEt3gcrsgH8by7A0sL&X-Amz-Signature=fab4281616e0261c9b3f122b5239ae5aa6ed96954599c921f68fbd5904a0d49e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

