---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665RRA7IMS%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T080049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEeiP3gumFtjXvlMyjO%2F1eTk4aociK875HLXKeui6oUAiEA7y1kiazSD2L25qmpmHlc3S3JX4qD3toZwAzzLKVT0fIq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDJ8P5u58mcYrLCl6NCrcA4zPTLoyJOzeehlNgq%2FS6CPy1sqZQfGmtvTw2Femf%2FzjwSqgdZgJIUNuOfOLl4lYGoEOr0vpe%2FPneKAziP4EBbvSeOMABtMba1iGEzkPPoHq%2BjpUIxcaa5LFOuGgeJv79INbb8QeX9GxItvKf20%2FqoqrcLMPwdkTZYqbMIsy14zeKUfV8kDh3WOnUM1n9opDCY0r3h7qOhe75M8NwCl%2Bgj8JrWS%2Ftp1PM18dKqW9zgeX27lY0ILyH4qsMERkikEN%2FhK97oSV2YlZ59dgeTIvShQMdXZ8FbRuR3brdhfvn5zYkqidHyuMb0j1b%2FEBeJnEWwDyaPoMyt9qWh5bTj7R3m3onoBua%2BLGBUDkW7MSHdczc1aKRWixHEHOOmfE9ouUrZKbzNYDdC%2FzMVNVuoryFBQRBhYj0Lb1HzINkrlWA3LPVVVgw13VySGJc%2BCr8JYNg0auFw7GCyvVUSRoDx2we%2BK6zEVuDv1Nk1zJWZWfPYMMdi61kitvA6Vy2iO9fBLuOlJYs3DcDIpNGVc8p6O%2FbcQj569rs7qPSKTvbltZOrpj3oTE6zyfYaaTGXe%2F8Bfaacjs%2F4ch%2B%2BAd16Oe86RazVYg4vSa0zGunBOtexHtC0zbakaUZiFIFnjdlxINMLeHiMcGOqUB1DsdJ9vTQIiZ60d8wkJg6fT7T38EzeeHKkGyanT%2FKLlXlJFbLlUhMojhbPD8xHSX%2BMSPZ1BPteTWnVtPFtgkD7f3rkbT3hH1kyEbnsVCl0pB%2FK%2F0DS2Hs2eSjufMUGiCb5zOyh4qqsBymjqi4tOjIIT6QLhViBt4nqvb0rLdCJlMbuf8vBnvSYnQaJptcOrz7%2FwJ5NkQB9LrescKA6Q5z69hOEiy&X-Amz-Signature=2defa9492c42067ff9df52f9fa5d304b7ef636cb8eb8ae896af670a0cb707124&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

