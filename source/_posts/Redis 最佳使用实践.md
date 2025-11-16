---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPKWOVDI%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB2kJXLTs8ATIfjFmMV4Qb3crn6bAU%2B%2BBtG8Yjs%2BbT6%2BAiEA4gRpo%2FHR0BILkt%2BvnBs%2FyMh3JMkoEQhlPAAphi%2BXnL4qiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCjZjY7WfqiLVNrNPircAw8dX4yTjtfJTm1Q8wKF5iwNVZ1oc0%2Bm6fPg9UaF5V2RWVKP%2FFqmdm%2FouvOHhknMy1ld5DEqH%2F9VXGLONlyEsNsJbfBX5R%2Bp4jwv8YC3lzBFFxk2iG6bi6GujsKYRwenAi4cJSadFX4NIc1smLK1YcHhwJTrtmQ0OxM8DJuSEwpb3BZtCVkbbPXFmr8EYAVasTIJc0X7S8BhvY%2FAI1MtXLlgaN4MQGWAZQbioURUy5YLegW63nyP8GWLTPLka7s5FriXrxU%2F%2FrvRUHDvvPYrJo%2F0IoQSA86KBAJhOt6xkviQZsqjGQT2a1o5BNPktSNojrf3vpw4CkWgYi3QHj2VKUNPDR11AJYihI%2Bqvss%2B1UlRKqYuOQgwlKhLc8lP%2BGc3PWRgl0OQ1FE%2Bp7X2PBBIKbhzLmh33zn%2Fp8O3vF%2BveQ%2BdJtjPwJCUqI9C05n5rKq1Ilnax7kcjKO6%2FYf%2BQq5bsSPgjslJUxN5u0PiQB8QWtRPP8n8CEAlaOhneqkBLoLOfPgyU3LyIdkxXzn%2F46SaQLOXiu0NzBnEvQMDtb0qqLPUGErye1gxzqNuy3Pwmusdhf2W3hD6ZaATa232uYRe2klHtvPd25EBKcSaJ5b7MbDKRRG%2Fhar5ghtEMzk%2FMOre58gGOqUBolU8PTyvlkqY1ewI5Lt1d%2BMyBMyDRgWmnsqxrH6Lzhij2w4om6JUftVmGfcSPf9eTKm31Xkyjx8fjv0IXdIOXLcZZHhRu599O9fTzVCPSmFjb%2F8afl%2Fi1LMEwIXsH0Fr0Q5cdW5Urz%2BOcdWLokx8tFcQRJlonTI5CwITp7Zs9kKCNVNRTVAZ4bKV1oS7jVHLvbBowtkCSSWkhl%2F03fxubBMFYCfD&X-Amz-Signature=f432397ae7b8bee3c3fd99c8547f5e5f64315a405eb750809cef664472e025e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

