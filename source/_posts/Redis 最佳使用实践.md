---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSQVCEJS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG0V%2FGsqPD8MpZEd6lV7LLxsg5eUQyFalQb7Fx6ooMbjAiEAi24X9RjwZDA%2F98P0zdL1sH5ySdHoSsZVzwKLOl%2FQhvkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDEnubqTZ2K7ywRIXBircA0atExx2rVlfna8QmzfJyREv7V%2BV7J0FzTK%2FyQOuLGuPFfA5eXyrlS5XTaYKo%2FzURfnZY86AIOKzPZUwVh8ExhtwRSx1YyYxNU4dndzQOy1FfU%2FrmL%2BRK43VZoxtFIb0uCc67bTp0v4%2BitLRfKj3mUCSdMX0NUlqroRwxTWP3VqMx%2FsbIaPbnn9Qcvr3djyXkXH3WOeNwHk74HCMuXB6MF1MpFQGK9O9c3blX9v8FFTEZVSGGRgLSwIPGFxGGJQ9hFNjGcwK01KaZY4wVsqyePvDBUVy%2B52D18%2FSzBsljPk57CZij3z4nkG84viS9QvmUIw9p4bGDRhJCmjYjWyMlBF5QUIYkHFQk6GCVvD%2B2sSV%2Fcdfx04s%2B8HdRC9iR0Ya%2FcoZQo2LLcNWKldrBgvsY8PZuZNCcJap8olsz6yPVIAVlwRgfj14bMsn9Bd9VMPBzznhSF96vSyqY04%2BVyy37bsPlwSNVnqia2315fi%2Bak849o66bLY4oYqekePJ8C9J7N6kEVQOeBVlPIr6y5PBtenURM4bNmwEy%2FRTLDWga%2BhJuvRKFf5RoeBRCxa5DtflE3aBCV0VjA0MND%2F7CRKepX3SxCImG%2FKje0DU8C9gdthJvC9tD9eW0Kqq3qlQMIbfxsYGOqUBbBI4J6RWcsYj%2F18TKKQ3MktO78xohInvDCMXjTsSwNeYMnwEJaPCF5oOnvOQy8pHD4kFfUyDJC%2BuhqIyWmeU38BKxOVmdhR8CIWIcEf%2FACAG5nku3rayT4gWqeHmSd11BF%2Fll4KlElk6o6TDhefHmIWAmoacOF%2FCEaYADGt4LgiHu0%2B%2FgfzwDJclK277X6CYpGl4QnVcfpmtnMFy4l5d%2B2u%2Bm7D%2B&X-Amz-Signature=e8bfa412cec170792dd5b6c8f9912d4e39f2e4d64b11659b13ad46dd4680eac9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

