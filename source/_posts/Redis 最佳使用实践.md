---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNRQKI6K%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIFjp%2Bic6SNt65bCR0DaME0DS5fvVEJPXNyzSinIA4VDRAiEAm3rdr9S%2FkBrQ%2B40HY%2FGWpeHPq977btqyhjRelCPh5wAq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDB6OTvD9%2BrFIO1DW4SrcA12OXcrs06fQ%2BztJMyvnZ3FwOcmY%2Bvzk3bl9v8kTFHgw5OsmDsqCf%2FZunQTpN8Pvbg6nPxs4OwYJR249FI2CnUkg5GoNBR4%2Bd8wXBExTlt5XPGqgsCIdUTaPwkzIrLEBjkpKTH4CZh5BghS%2FivN%2B83mFHRnsYIIlvoSNS25GkG8ORHvtw4l%2BFLVEn9fIlIhtZyv1mHKrh8zke1CxXSe0%2B%2FlbmJtwoASRDKkU0ZMws7KuGUW8MPu7QmRVVWApKf0imEgTRxMYZiWbtezkWMPxsumvGaBcBO%2BQlEIRh8wW1BBQba1yLZ3zq0LWcwsU2gtM%2BO15w5Q3ij%2B10S9AmR3jjEuHo4kmLNJW7r1C19mYay5%2FU6gOc%2BWKMf2qOAqbZuom8jf1FnHcPRTBE8a9KZZc3Kc1lnLTF6T8sh%2FtTzcRtc3M%2BDqAUSaPEbOY13a2saOfIgNVA71MI5LsmYl9MOZ8mm30GWN0PRk5Og5E3ddLwddxMIZzs8OHk2L%2FTmdO6KLc7wGBqT2ogKJ1VRUVM3XOwREV1JgwJ%2FwKZ6f4oXyiqKuAoce8ur9BRwyIKENsHod4Z83mEPlOTJ5wh7NBQaiRTrNlRluevBfuAFPQsYm0MVEifYgQlpphyvzAK2bbMI%2F%2F08gGOqUBoUVHejn144w4pQ2qqYMv1QsyTnNnhJm%2Fig2dQk9U778bf7u%2BnCafNi%2BOJNSr9TuPZz2OT6hCBoH4oPGhNLEe6246oHPhsD52kmdCf7kDU3hcQVTZF%2FCxIMflOwSDaUUugJU2hqtXFb3RWvg53skq4JOuViUj%2Fs9yTwhOH%2FA5142wuUrDTW918zYMw9fX0pphMcFj%2FNj2%2FmxOHI9kwWWMRuj1pkYK&X-Amz-Signature=b2ba9c941887de9666283e8c9c6f3796b98c8fe6b39447e9cceb9b37a0a4e069&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

