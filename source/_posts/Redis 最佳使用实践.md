---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CL6VZYZ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICRe%2BHRFklbaHxIfnGdJCGxh0NUWI8Y4QPJHIvxV8a6EAiEAnSD%2BUc9auSfwUSqYDBaClDTwkrXpuhZGluAHrt1rnKIq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDLAhI9pY31hmgPJy4ircA3aaVI9VkvLsiPY%2B3MK0h%2Bt%2B%2BONunnDL2tC%2BMoj0RIHZgJVHw8WeZsTgi5ifKE1yAToVz6%2BvKIL3yficvlEN0xRN8VA2WTVN%2FRNaa59u4TxvLM5BlSdM2GvzpSTSCL7Wd17vFgp9tHuPsUaYvlAsxC56MLWmtzDhApTUWfe4MnAOsET1WbJaPDNjuMpIjowv9YYB1vhXoONUPgfJf5gEj42qn5phHcWlLDxrNCAdP9C0MBjJUo5RmmCmJ7Dv8ITzGcfvdakaudtNklmv0kqkFrdb%2Bsl6JheXFlTbYdaCW%2BA%2Bpjvj6tHlXcq6%2Bj9KYx52lX7WJFan135BIi6EyMSnJlzBE%2FDNX3CSpXAswZj7KTkHWInaq%2BtscW1R3FBbo7wUVhcI1oNZFxWKX3ACJSTmkkbvm2qwuTAKHIEuh2WiE%2FRQkE%2Bupc9OeBzJOcUbBojFh1Giz%2BjI65jCdA2kwwZWWiVK0yDj9K%2BvBZE9mymmJNa8UCEzZVLHoiF%2BfOGrjmLehzre%2B%2B2%2B1iHuWFklAMSGtvc%2Bs2wMVF0eseSwWXWP1pPSpiPgqKHWkRKpPvMsK81KhIjet1LGE0uae3VqxXw9iuXChZaYaxND7313OXXlJF5PwlvdBmQ6IPpMNYZLMPOEv8cGOqUBwpHrYDnPDGv61%2BobvSOQ49I%2FVtKKkHUNAKkbKVKTQb7cX7MLNTqgWdE3RO7T60bo8%2BThGuyEhDIxeWG%2B4DOersRkS09hL41dqQxvpvON0J0u6%2FScCycaxGC5tb3chdoWBvxb5uIzT%2FltuGUdmndGB%2BTfF5N5eEKfYLVOMfiitT37nzo7mE3G7AkM6VO%2Fz518zSNfgLYM4snCdz%2FZchdHJc3URmGz&X-Amz-Signature=6bbaf8ac0d7b13099f0629af82d2c99c7cc84074696ad4a8ffeb9072992084da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

