---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3BQJDTR%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T130145Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCwMkuIVNE%2BJnr%2B8Es6iA1LAR2OL8o4gNsL0KY8thLbogIgDswDcADc9aCRroRl%2F2j1HLAmSEJZX7QD1AV0d5sSG9IqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPoB%2FPIpsbIa2v0f4yrcA05F4jqholXW1XkJ6OT5iNSwIjYGo4GiVGhoD8cc%2F0k%2BY6Nf2yQVSzMXyF94VsIUxa6Lym1%2BwA2Vr5wYEb7lD6zOAjS3Lp291CsxPcQK4Ytwe3wP99k4XENxe4qO189F%2BkSxvmrXNMDgzUXSadKDa%2FSJMW2y00mwbF%2Bb9Ek3es%2BjDMP9SrqLUdplOcwsNp0Tdcszo1Rqd10fTEvg%2BmlgzMH7%2Bm5lQyoiXeOKfHLShlnUdM1n4LlCAd7CDCxhKN4os0SGHH6JW8G7uYLvNpZ3MEhIlGAq4%2F2RRANNX9X3ePVVkae1mPYNtLPGJz75Dgv0y3T3HduHjZ6Kgns4Or31UOnD5yuCfpS%2BmIy9HN3cdaj3CY8dBdKcWRm8WnLrue99g8r4dfZJvq8MTrw4RU%2BRRRezw4np0h6IfOySjdpmBYjTc4vAnSa4jaKhv1RzyztOYNoOMXpq5sCW3YFdPdP8MKUUqcCT2JsRq12XAS%2BAvGYym5OENdW9jSR0KuQyulgcxrlyCZVgIBFljKFNlAVcv4POtOCM3o3EsWt1%2B7kr9bdHAgH%2BGIlpKFoc90nu9hInd1%2FtZ0Ve%2BdD31Gxt5eL7YEobXofFnfOOk5aWceXLe%2BN6ds6k4GJRpggdYZv0MLjar8YGOqUBIjuh6gU5b%2BhFXEysNT580G3d6WwcClKeqEpZBjU%2FSvay%2Bwwq3%2B4oRZ9pjnJGISWjiDtYGOwgALhmdbcFq0N9Nt8G6wOA3C3HdSxLK6aW3uW06DFroNVxzfc5AkSPmltWe%2BXxNJCONZg81S93seN3IuiEuu4Fq7Pf%2Bf%2BdUqV1nQaCaCbX%2BQM06ahVXf1ZaQJqXud1HuLNoxpuSCDQ6yRkQ9%2F%2BOUBv&X-Amz-Signature=9c2915051b736bf61bdfcb1f628175bd0a28161908018ba71ea1ad076adec0b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

