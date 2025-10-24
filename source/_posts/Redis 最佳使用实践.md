---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664SM6LKPK%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDu4pf9jjgCbe4XhfYq8gv5OYTQbfhHx%2FI%2Fjg2n1yM05gIgWKYW18lg2SjujrJ7PnD0scl7MmiSQlu3YRtHGz2EyGIq%2FwMIVhAAGgw2Mzc0MjMxODM4MDUiDDhQxaTCpXrHuDyegSrcA0iyfAxxjOwBJ5nRSVamU7XvJihGNkTJr%2BosQyCLmyv%2BIM%2FCyALqh%2BDuccX5fiCx1lp4tFkV34Eno0MhLlH3XcubXhaWcuUa780SQfngpRmQH34q5NtohNBRW65KV%2B0fjXr%2F6RIkLnNxgu%2FaNLh%2F9EGZq%2BYdSMjUd5%2BGdjmziexBE4C2yvuVRiisFNjak8xWaaluDB3Z%2FUa0SP6iEzBO%2FxUhswRMmRm3y1XhQCdPH%2B68GLBFRqXR2YG5J1LxJaSDPAbDsXzD5VnT029ffmwxaJNcUM7mTA34z5in8t0wCkXHVuXaoqY%2FEnlKO1KqIQ1mNyXcsR2wgmfmnnVN%2BINyRaZeFbs9JFX6Dp6eNLCIzgf2PAUzGsdfcTBtvRWQEnZkH1xMFx2EhyfhOcPdzdvbWet9cB6qPotD5rQOMme09cJDiHsjWG%2FV24Yw4ZcG5JObRzT0JLh39pkmkMi6lRxD5Kc1xvaOZHwrtwvoEjK%2FIob5HBWO8i52nA%2BH2wfUVEK5NLxoe9fA%2FAYWstfENFq17L2gocfXRBaIgFgGr5qSkLUhDxZburqdmiNRTK%2BqUljbNiiovKLsj7Y5jrfupUPGHO9cBonq5RH7NrxZKqcARAxSIlara11kjMWM1H81MMeL7McGOqUB0uJFZml9CxzU0C9WCsd0fn1T8jced%2B4XtTP%2FNGatp%2Bx%2BseEffmkBlzmDFLJKb18SIRt1VWPSedpKeCPn6kPA9UQjJNA%2F5p7USFVSo8tiwoGPZutqyYyP6ewT%2FMN8TgWICyQG4nFlYbz62gfk88UwqhEEESqztLbUrzem80YUXlBJ8ldUZ0MuSxBZGCHrPNp%2BsJlFjrruKMnCK8EPJHgg9HIq0I2v&X-Amz-Signature=339d9aa02c0eb23a1fe759b85799d2c170dbdebae8c907cbb6f6aeb297076372&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

