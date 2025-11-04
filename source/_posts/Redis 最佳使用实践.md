---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UUEJP525%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIACWTek9%2BjOuX2CgoZtXSLqSg%2Fl5DtJNzCM4z6c3D9w0AiA6j3KgUWYCCQlssDCtdTNG8yuOGTH0yRGPzrfKO8ZzPir%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMndUqGPujPjgATe4vKtwDvLClWVzgw7tU0sD3Cs9NQE9zUEjPEH7h2riWXmP%2B4%2FOzpcG4yMTEHcNA%2FuSBKFcaFeZTl3I37Ljwo%2B3LHq33tz2zSvv14%2F1jrweQUxrk5NJpPc2ra4FC2ASecDM2xbeQ%2FyAJN46PzFSwSkJyFkOfvKnAwo7QuSn2aXsuW8aCR%2BWFjnRBmikhwp7cl8%2FpTtC0f4UcmBlrU2IJGuDLuyias2GBqrMZxG5riBEEz4UAD6EmI2%2FGk9It5DaW4jqokenS%2F8OZOZsZzBjBvau%2FOA2%2BxArY1QCRA2kTvfvcVTg9orpbspgKjTnHZuQef3Z9DiiIP6Y92eZb7%2FsXMGPvr6fsvY8jVLH8Vl6W4k5cWX%2BwG0tHsGPy%2Fwsz8haRady1TTdqjtOWvM9trWGwVD5vJi83RRxgfoj%2BA5ThWWz%2BHa5YOJ5UHsyX71onrJhC7ATbDHwjADinsDHgMVF4DD62XJy%2BNDFPb8t37OasWR1gqlqy0C20PVUIg1P0w1MVQfcltrP9zldiejia5DGeFhNEyBfuQakfv0MFSeO0aXjZFZ4APFFVOnSj3ou5wzuIFcQVD%2F8hqfYzRm2yJyVofW2k%2FGdaelEISWnEt7ObdvxvRhCuRePq0JF2sjQRjIDUESQwwLmoyAY6pgFJz%2BTp1CkZVO8JTTyXg6TwVOQ5ZG4AfCWk3YGNLVTQf8907c%2BnSbVGIcar9Ab5KBIjv%2F7xidALWXfN3CUkzGkOCUv%2BoWZQPvDtezO4sA1kIhqHYgYzy7wxmu1EoU2wbVF%2Bom75YyXYp3Vp6w4vdIRSl1AwrkM08v1o0eSu5wa0wTdpTcqMGJy9lQzAwtqIKXD3zTxED2iZyGkMJ%2B%2FCdwbSSHkCvrda&X-Amz-Signature=f7065ec894189b14e315d904ef7b8d3140fd7972831373cee67c508ac5f3d238&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

