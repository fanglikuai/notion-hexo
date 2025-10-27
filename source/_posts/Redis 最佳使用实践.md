---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMZ5ZJE%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAnKsel3d1bdtDCMQ3mYe3U5tusTPLyRLCPzgvmRlMwAIgHMxP7Trr59hfbFErfESfXc1sJ2t2AqxHo7ybOe%2BrSwwqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB6aiTvqPYNMczj44yrcA6nHYTdKd3Z4eGrKnzoPdV%2F%2Fp51ja1M0m9rAeTLxlk69Bw9KbLkxu01uSAovSL2AseLFkucvzqZbajkxM7MdkL568GoZvYS7e4WtNMtVi44XcUlumAgvxdkdVn%2BoPaz%2BG%2FTlgP7Gfa%2FMYCVCV1GP6P2DRdXba7zPVTN3nc6uAv7geFXh56JdzPhVCkRNrRVOBbl3hguRfBAtIL7DI0o4kBgVukkW8aYw4jQ1SwjiUtUZEpaV4zyvBq3vGLG9bvCDeilD3becZF%2FuRbkeUEdTKbaq8%2F3kJO0vXdznTj%2FunqE%2FVJTFJgc4KtsuUmhbXD3D94fsT7au72CkvwWhbfBJcD416%2FDlPILCbW9YM2GIaGG%2Bxysfn9siPn71kg24ulBkFTwn6hNQmw4zSj9YwZhemEIWjx%2BkWQsn4FFyydxfiqpTvuUV5%2FE0T6T%2F7v2cA1XwjzIBfTTSz00AvIBJMv2m2YuhN0rbhUz6dF9FHlhD4dZZIs4MMozE%2BesC4QygNrQReb0nC2MGA%2BGjlQpeeb9b8dMrSP4NqXVHbg5Ip%2BjJK8Bg3CzUahJasbP2ct8izDiz%2BaAJnym6hWn14BBuwu9HapH6YY8d3Mb7q6VpffG8ThyDXcQqy0uogERI7m1nMLD3%2BscGOqUB0hEmwbMxaqvQRaWBJCVMFf6LwUwVY6KWAaml1OqAbOuheKEM06Am%2Bfk2voN1nPEVJ6FzJyTOiSn0em28koHdyUWzPxXB9sj%2BQ0%2F%2FFVjwDkRdAgvUgxvi1qr7NHzkkGJzhOxki%2F5EMdTq8O%2FICg%2FJhyoIYRxw0%2FLQuEtnZuPiLL0KMScMhKnJFaAsAlaoOUY6LD5%2FqoMGsRrsIAq6irLRVhPI4z3l&X-Amz-Signature=446a16605889934539e08f7c0600900b41385029a0d99f35116957bf3fec4ac6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

