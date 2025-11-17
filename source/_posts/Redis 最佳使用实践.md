---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEKPSVJ4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEji3l%2BlpL%2BXgtBO1vtn%2Fn4bhZCg4q77GSe1%2Ba5uLIDSAiAinpFiLUJJ3kY82KZy4EubyB1KuNHXSxZtCeBBjk6%2BYiqIBAio%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbXMi0I%2B6JK80p6EpKtwD4D6if%2B07bsrERf7PTWmUXEDVJ12vICyub5lJKCUCtB4ZKw240Sg4hsBmT4Uvk2Fajez8UWUf8knrmXJKxkIqnt%2BMQsKLndu4%2F%2FhwHpknt9RKpRgQ7Z0pd88FSwhpiDEXQtUtCBies1PGtqRXaiX42O1GPwcB2ABmtdiIRKSlMg7fqXRt7EXYV4SdQL95H%2BaF91J9QzAZTyx%2BcBf9omizSzWB15N%2BZBDBhPtvEFAZlV1DXZLe1ZPxpFw9IrxCsN42v2PpKVY7WJ3Q0pzPnC0yJWzYAEMLQ0CsW2Y023Ib18njZTHtJmC3Ye0X%2B1z7jBBLWeiLSJhemsSX9685i%2FIPR1TUvOTJkFcfJM4zQM5WMHeI%2FJdCOTVdasUVIw4DMb7Eticn8%2F40I%2FwlTCs6ytMXBMw3KknI%2FqZTqOtPYBOGEc80jQsetWkoRDDLrzkhra%2FRg3INznx5cxW9zExmrkjnytN%2BDcYD%2BkWbnmhbXwx8COlHDRCayxKJJd28FTu3MI7hY0Bpxqe8rtISv8jpaUtrJ16JQHpUk7Wf3ChdzzZ%2B1wEAkWpCCDLghoxRyByGZuMAaVA8PDYGYTqHPw5FpxT%2F27tMKfqUHn0QTwsH1YGZ1YGGPgjnzMwv5jBOQP8whJbryAY6pgFWUT7BmMKJ05BxjnqIsOtIUlJUCfG3fQYt9%2BGZl0%2Bb2%2BGSM9qtv5omy%2FMA3LxWXfmAno4R0DqWNOilLYAy3gPLC9SL78vSEMmLLJymEv0inT5a58cV4utoup1BiYztWh3mn%2BtVgBi93rtD6Zr%2BUuVlbHyAnXigEtIlP10U04CjPSlYCe4nVOYK01WNecx1LM22TSFuXa0MjEzKsiu%2FPxiJWX7HLAHa&X-Amz-Signature=8289d30f5d245a0fd5027176da45c7bf3cb5219fd3ae513337d13f43b040186f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

