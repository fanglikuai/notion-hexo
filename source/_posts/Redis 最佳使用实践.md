---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UN2KFNS2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7D1GQP0FNLwy5Soy6Wg0g2l8PGw2DQ3nAgTtsEtLu6AiA1yoRtWNBP7Nr9HTfgVLoV6bprKsk4Z6KjOi9UXH5dlSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2Pjn3AThvuQVYglzKtwDjVJbaMEThicEwG3H10TXbetT5y1ksjTaTUQQIYobQMWxEM210XBShHxsKCLiDWDHyibI7JVb28Sti4Z6FGHMug2UPFbFXB5z%2Fekp9sXHhgP9WLKAc8dHAhpp9ofaez%2BvG3DH9XAfyYKc0qiSyEry6wal3cqt%2BRUkNusDmvIVOZQKtapyi5TqgjKtIelMFCVWr2wML71R1AJO8Cez7K%2FMZm1GW2s7AVKU5sDDqJK3G5gbPS2ZJD2GfANu5WyULo%2BeKUqejgJZG%2F65VghnCVYsnDNbbKUhF4b9ulp8L3ViFtSsKt7zfh71ykjEmpi%2B3o0bRSSq619veb7Or6UciXOkIwC391Na%2BiUd52kodwU5ixdSC0%2FM8SidgUuK4jJjySbcxq3rKKX3ah83ut%2BXqECQHg1RhYbnWXTXuLbJO8hCVac8nVp5yJZqG86jHGi2QogfjiNaN1tv7BhprcIjUce6UAiGaEQTUUPI%2F6gxvkVEJM%2FVvjuLd5g5ZW5PIbA60AVfvono4IUB24gB42l0lwa7QPjotVovCAZ1XjdAD%2Bhk7sGmLC%2FthwSl5jrHjb0AC1Sr2Q3Fu2ymPJ97E9tX6Ey7T7W2mMxrnUFEYvR7iI8BduoVyhgdDMpQddcPdUIwstavyAY6pgHjhXiJfrBW9jPtem0dpcVJBgJLDiOGW%2FBnJLvyMZMf4rGtsv2y84bIyRIbNTZGpfNcpoik2XbW3TyB8svPJZ3mjrMGFTlYt2KAa7aY%2Bit%2FwPQVReVlLpTr6JelTdXFBCqqyjhuFiEeQCmx4SWVOPZrP8dYDvZPJ53dJbDjKQiLjilMIALzKTn57P1fOw9KFMvfmcgIamT4TY%2B5S9BtjdfiaNEhqvY7&X-Amz-Signature=6a16bd2f351dffe8be041cbf4618f0e4a3a7da6067bc308923c51fbc6f1ecf1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

