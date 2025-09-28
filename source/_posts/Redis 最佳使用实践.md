---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIMESF4R%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCpSzF8NUsX3ZfEtZI57I1afX1MxAQY4Fqa16UIpzTZlwIgeRmTg%2B2RLrREcgGmexLGeOKMijiALX6U7%2BHjjqTHqJcqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGDUPyxs6fiXNt4AByrcAyimqL7MbTVmFbLA9DKf%2B%2FOAvhUTSG%2FBmPwUnNOx%2BERN1qrM7XDSUvFHciBaI5gDpENz4gzZAxukGHdrDDQxhH%2Bwg18KawGM8z5Dj%2BoGMxrRNsg5KxQsdeNpJWEyNov5RECVbR8ISmVXfh%2Fte60nQIXgCtajWm2ibuTX6Un8UbiGWorPLxnB1BFAGsFkraqvkgoyUa7NkPvvyvMbZAhTvQ8MwWixn37%2F7X4et46Uesu0SGDabf98HOCo8fqVIhCkQkADu%2FHcCH3muQC6fcKms4V8%2B03DaackUVY04nZYYc2wFyDR2%2BE89dRrpEB3ETPvXkvLp3HTQAxuFr6UpTo4wZkVKBN5X1S8485q%2Bh77nSPWo8AQefOzplpiPBTEZMgUSOXdijAhHbSbJUcge4%2BkkvtrTfPoYZVYlXKATMIVgiwbq0QdK9QdYlNS2rszQf9LQlW4z0YZ%2Bm5Pr9JwKNsxwx%2FBMi1aJ%2Bbt56QdMrrBV2VAwCYxeaEnK2heT9ctznp6MhHogTwrYvuwpKh2djjoMzIOSMp8NwydFq%2FoXu1ccLg7Adgg1YnYegPxfANQ%2BeZBCV58G%2Bjl3TXZ2cgyjfP1CmLU10JKaCLBLI%2Byatkxs30RxDpR8H1DGmiKA76XMI%2Ba4sYGOqUBcp5rilZCI1%2FfqJRFX1hcBLBFGfKCpKLY3ZNtiy2roU94T3iUhjDhCZCWXQr9crdmdPdnDy1t1Hmw97zXISVJfj8RopPq6Rqt2sFRSl26FtIsxgioF3t9BJyF7F5cvIiQ73a5gIAmiobOJkbgXiSOgbVlN4OsWf2FtDtsnwY2wsHhqmtfxTh9X4bGMEDUeMapbCr2XpZ8NKS8sl3iu850PJ3HJSog&X-Amz-Signature=6dcbf8eddc77bd024b24f9566ca1cca6caae25d3fdc6b1d6391512c473abb584&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

