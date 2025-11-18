---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMGIVV5Z%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBrfDqUC5G16IhIT%2FCw6%2By7kziU4ijaLGaoYG6%2BSJR1KAiAukL42ohyrkDWy7ZWSrSmoj2Cdhlk7mB3gyCWQO7xn3CqIBAjD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FaNZonTpVw27HpXsKtwD1Taw3JYSrzW5AGciC6cYHv8EoEb4oqlWBAxMcc7wGXwgKl5Ea2vpujbt3%2BnjkoayBO%2FHKalwNQrWd36brdBotF7yukGCVqNlGHgMdtylAF13p7QfboXJTwhcas%2FOpMQ8Cr%2BI68hXGNWq5ltr%2Bn3s7G24AGLn17V97hIv2Kl7wA59IoJU5Md9xc4ckL08KHw6CV8jKRtD4%2BxkpYq6m8RgsNaDz2F0Ab7lZqRdM5Vg0BHrhZNscA4K8CixwyYfAg1c%2Bt612ruDKGVYUFUIR92OF%2F4gL4BypKqkg%2BiNFskBs2GR4g%2BugaEpPAoKSIPG7mK8h%2FBbqW%2BVm1HVVVu4oGeerndZm2v2D8k5rhW7jPYoA7YlfsJOWuNvBP8%2BzKntNVjLDotXU%2F2tnZPxi8rQiW09QpOgS7LXu3rBaEAFPdw4ZyC8sNoUEohsD1CRMzeViME6k8psnU7%2FSySSo4eh3B%2BaAB0DQX1BHsITMe%2B%2BGRrcKaumWxZ1mkUPUexySl5g7hxcuFedtu2BXb377IOOWTlvXG37x8xyj1OHoq17Ysc62tY%2BrMqYBUq02PFMVz6L3EaCV9kW9gu8QSV5ZHi5YvXBbsECCD%2BG7NTV3vqRzmjJg3O5PAH6nIK%2B1Ze0v6Mw3oTxyAY6pgHxB%2FuRsCyliOQulyT0s6GoiAL%2FNnY3PVjPQqhx2gOkXvXCimsY7VBKgGl%2FvYje25oUIF%2Fm4Jd8KV4MhC6hTWcgm7A%2Bp8X37xyC1FLonG6a%2FsmoArJbcWvcEJ62I%2BaREEJRPMWKLXCdu3UQeDkm9mofbIXdIPRJmjnmoRocfFM8R94ZNMdmw6VYiS6r1z2kXuPBc2sPYLmRJT8qyU6orL%2FQmFTOWntG&X-Amz-Signature=f440a35ce45f30322ecd25937646f9a36d9b21ce07956ba226bf295d5576bfd1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

