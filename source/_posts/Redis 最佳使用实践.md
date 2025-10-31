---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3CZYFVH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDhuynwHjY7bQ6GFwwy333G8zSTMzZrb%2FW%2FwUZVEzwsngIgcU4WTo7TooaZ%2FlJAKfD0WwHFZ%2BUq9Clx37R5b8A7PCEqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGSmHzM7IyKdcoCKsSrcA88s6jkSJDh%2FtTj92CVqO5p7untDopRV2s34IsWZWg7TW3SIIfx8jPuyFsI0%2BBeNAAm29v0l%2BUebr6TKMXmTfhG8tecrGMXT3Tq%2BUojhz1U1ybgys4HIfMq1uvpJujdeE62CYok3XAQTxD8UHImYLTiGRqCjDyEGdkO03oSyYqqKC3Ew0KCx5zKo%2BNrQEQaoaqx5B6c4kAdY5vH%2FOzXK2A%2B0HWve7Pmgm4KoWhGUSQ38%2Bl%2BsaNc7RR7%2BQjkoPN5TYZgMoZuiPTpp3A6H0DBZqc%2F4RNozESqi0mgjFaXQDt5Eda6wztc9Rb3kcqhsambaArJBxivKAUKCLmdUD0AHCgVKGA0Zd7E9sA9sBntzdXJoNwCzMydgKdDBBHeLktRiAWPTakS%2BuVRbrFoWMCPyJDFW8MU0s%2FhhWYkRqzqpa9DhHvbVltUFzXJ2%2BcJKn92QWW%2BYBpHFebORg8sSOkRQ6KMwnRK4CE8rcMhS5veXjGASMoVwmqSrmuYMfSEqeSHi%2BM6xfbsGALwd6AS1Yc66YZAgpoVNDZItELiYUcaGaB0U8mIO0%2BUdw0Tquil5ruBKWG%2BB003PDSqcGFneX%2B2ByMrBKY%2BeYqzFVIE2Er7iU%2FmC9YrdxLzXAodXLJQfMOqLkcgGOqUB5dCJZFWmBXApG0t%2B%2FmrW1EH%2B%2FnflY33CjSiv7UBq9%2Bf72em1WZjdXgMKlDtdQKP9t2w1nLdTLfvQO1KiSFxO%2BiQ7ZHXP6Wnp9f4IzO7ANjSM6ocniEKbJWV6GPooL0cv6hZmxvLc0qlXFdlbi7uXOw2fgg69RL7fB3W52UQZHm914C5x8ufweXWA7pPqFW%2FjhI8o7I31nkaopacVs5Ah%2Bjp%2FEjis&X-Amz-Signature=7ec2a8d08aae5458b0b4ca9b0e77e7beb92ad1f27b562d3457cec7660f13b112&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

