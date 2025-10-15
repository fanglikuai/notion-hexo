---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNXDKPJI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T200042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFzHRcAvAIOJZEaBG%2F%2BAu2H0BmKDF19GESDgpaiJyiY6AiEAwZ8cmM7Erz9kp64T1376WkD9y3NgR2q25teL01sRdvIq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDEgMw4WbstlzoYjqTCrcA0dcdcG91BV8p1KgEsEApbiE96wlgheq0b9zjLCZ%2FdaUyffDaQuoihmnzO5Dz%2BRTHvHvkypffqkrW%2BDOTV5sTjmKBZhsqZZblcG4CA%2BjYTcmOBnFY4vJUZRqdU5wA7BYAmL3rNd125yOQ%2BuTGUOL%2FSaA%2Bsebb9RiwisUf32yd9jfUCqn3O%2Fv5BwC6CGGlit9UwJpklURUjQaT9dkGlyqBNv3bLBFXp7Dsmk6%2B3r3ImzNaiJvaZu4ki7DLj3k6xjCzF4EJH1FNunJToiPaInHOQoMdSXKXqx3689a%2BY40xODL8SrQU%2Frmj8rumpupWp%2B25sqwdCzaTx7zU8SVlwmHgZWcdqzQ57m8CeEC1cLpHNfwFfkPkjnUSLHT%2B3Y5acSCA7Y11NrH7uiQygemKjDx%2FMqtVzDOyNb3e4aC8ugM01DQlTCMEpoXC1FEUBmRc3PmSvVyDM5RtIoH2s7f7hWQjP%2BTM4YXopSk6fHoLxxtaMrr0iJrxviyybiQFJ6gQvxRX%2FlBrmqXnpAE%2FVmVISrqpmva8GzB837uZohqMg2Ijaz7GC1BPMMq0Wgki2bwx2wL8BlgjEDIaX1jjfz3HGROjASBGQbnV5%2FaPTKnnao2%2BW0Km%2FBE%2F0%2BkJPmiVUhfMOfsv8cGOqUBP0lp61GNEYyXcYl%2BYR2gw4OvptaqhZ75eAqlRiWFJQvbi10oF2OdOGc5fQekADl%2FAps495OSds3u1ZcjjTFxQFGdxvSCqm93iwoCeBFLBbB4aXf%2FF6UEBo%2FhL%2F9XzwSlsSB%2FCndHV0ZBJVv9y1SuVydOmq0taY6dGAHNSgvQIGQP%2FpLcVecJIY3lmYuIBgDdm9z%2BC53tKcxoiu2JYt5qq%2FrZBUib&X-Amz-Signature=cfbe9592581f08738bfd4fa9a94413cdcd414ea35e826854cd8395eb190ad9bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

