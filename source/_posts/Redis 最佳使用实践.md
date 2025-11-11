---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652XGF66L%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIQDnOBBRp2lX2Pcy118ZJsMJtHXrojDbQca19dxJ9%2BEmzQIga0UQtzFVzeMw2IAXkZHwIUgrCx1M6djsy77R69pEE1kq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDAQg%2F%2Be6z6gH3kF3HyrcAzgw88GkGS9RmlPRMaWg6MRZ%2FLAEJyQQoLkJwwnDJaos%2FjC%2BUieVFtuPGKb2Wug7TYsiVlcq581Eb6OyGhQnOxR1PZqdOd%2FkLGqwgz13m442Z%2BIHWaYJ6u6B59RKQuZoNTX3NYHOvZcbhcSCegaWoe1U05L9O8FBRISuw1O3gfcZinIFaydJ544%2BUmUXXZ1Czp1WYhKBx1SNfmvnkUWDdisc%2B6ujrXPSMGLDJwkwB75JC6mhyvkqf7Y66FVKTePQ%2FTa7Ed3WAWq6zFOsH47ILoLewhBAWwviXrBaH3%2BY%2BFSFJzGAj0Jmm1NTFNV5s7nObq3aPtpxJtXcGQuWM1e3aJGNccpFB%2FSDUh%2FR%2FPeEaaMhWI%2BkVhU1schZkzib5OZAwhpMpGzD9p3TuoXp%2Fe2Io5kTcSvVRR9U7K2nn9%2BCkLEEBgWSdvX09WzqqM%2FvJa%2F%2FF3e3kl%2Bv%2Fhaeuy3wXa%2BJ9kfGOKiNOYNfesHl0%2B%2F3oN%2FONYA3aQSxW2RIYTZLnkf5V3k8Z6hwlx4XJb%2FtC1Yd4w9JVo7mMqxt2vGep20SrdjhEECH7rd9uGemURggikA%2FL28TXv5zdOJMwFTyP4cg9bWzDecIi7NnB5XXswp1KuZRsSGTOqosuf2qhjOeMPLnycgGOqUBzqj2o%2FDqzZEBUH6HIZDwLE7YZti6huYLzxVRapXGfKYE01wtfOgSTguPqklVsAhsv12vPJblVE5sm%2BRza8fFM29FSHQzEgCNwEQnmpSFiqCoXVEZmkpnkQ7wSMxO9vUWB9L4h4peaNXX2VH8mT5eZffFhHn2irwBiJiw9nen%2FnhlZdGSvBIV16MWFept0u1UdWoT3VFci0eS7iekd6Mp9XU40z3C&X-Amz-Signature=b0932468bf63f80dbfa787c8369ac3c2be665b9f68bbf6699b52eca2427d8ff1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

