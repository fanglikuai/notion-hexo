---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WOVZTL4%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG9qDtU%2B51wYnHSjkceno5dji8IGU4dFax2VFPwbu1hCAiA0Q2TJQhjMZJm0bZjn00YmKM1ac8H2mYwgD6urRtDj8iqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMezduoEizk7MgIjYKtwD2ArEVwMpX8KY%2FK6nKeETy2NbYsNcfvBW3gQF%2FeJ7kghjtgoHPbEQPhN4cmf%2F413edwQpcwmeXTo5ILeZGR%2BhUkHEXFpWBGaK1wxwm3a8CmicrWx%2BfUT75CHCo3dYQiP1GhRxkQ0h5QHqqn%2Fp6v76Nw1bEWnCYD1FTcS8KiHb1JiIYb71JEKsWyRE%2Bxllu92I7GPoVKyDGZ0Ftht1mN12Ng97bFHTKrzFJr%2FtnPKbUiKfx6MMO9fbioNT9ZsosepycG92%2BMBsmTpw70fSreie1NFHiN9h5PjqJ4YNKLfnpP5DWuu04USkdNEYvBfPb1PDKDAbPMpRM9uj2f3oi1lS0IGIvgv7eRHX4uYLUe7iY5bMCB%2FAIopZVecma%2FCnoWJpbhdwII0xYNTekc7Gkb3aNIICKF9mxdbse261qIb4japHiTDU%2BBE7yD%2FdgCzQbl2vEbmKYU9ShSDQ4kQhuRLzC%2F2kGCvNw2scFsx%2FGlmzTWvelynWyIw6Hv0EOdBKpfJ3B%2BjPp6eujm53N190gq8Mhh04NZxtRqs4o%2B5Ru9fKT3tBOA64XTcrA%2BatDJ8iWSPxjy0bqY%2FHX7PiXZrSNWz%2F7MvhNtlv7ebg1ZGMqs0nlqdZHyeWPnKJiuL3m4UwoaSyyAY6pgGztogYlANf3sAWXviBmo5OyKnOe8bv5v4nTnjYkYAaPQQqfGgbKNAt%2BDiwOOwIUznpHIIweUiYsQfMqHFyBjd7QdLGCPvOvoJ2G1qJIVeERKFOd0D0p%2FKCDC3kFsMk%2Ff7cwoRnM4wlrlibQAJB%2F%2F4RQZw4gV3cCmTPAAC5vTJvjT7z9GxkUEYcE2tJUjiJZ07EFhE8eY0Q8BHeJVgRcxY7%2Fe2IqLHB&X-Amz-Signature=42641328e74b822397a1fb488a991624e45f39543e7490b78a179e4b0fa221e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

