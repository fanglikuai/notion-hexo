---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JNY2CNN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWsFCUQGzAHguwgdTqWGtf%2F6sPeggUENhOCRfWeDhRmwIhAOvP35mY5XfybPP2j%2BNd4VRGYwhnatj%2FmeOtZ%2FxxzOQQKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyGoUVOeDkQkrd16fwq3AMC3sd7n6ynJ02Jy11c0HZrTWmBQkLvqxnGFpIXJNz2wZF8FTWJ%2FTn00FnmUx%2FsvIjqXI6%2Fr4SSAVOcomo3GEaIVr8srkorWnG%2BM0Bh3CLBu8SGilwrsCG%2Bd8rezAInaF9u9E1MQRrk6nekw7AQt6d8FtkKyo2RJ1DLZwR%2FnZXiWwd87ExiYd1TFxX17u1ItHHfCE3h3gQqgCU4cplxbHDfpiex9GMKHY36%2BpYe3jiXldi1aOtLaV4TV7Rhfm6YRFnz82db0TdoSKWQSPLBphpn%2BAOARHHvL0PRzGlMCEAZZg22ln%2BvM9HgEkH%2FkvxEm6%2F3ZpH8bABcFuYCiyYu4HvHWHYuUXcfehre8Jze0DwanfUvX7yOMJ1yq0nNsVGdA%2BJirmMosiShwLGUxJWYaIZp%2BLbSsGNzJRRzkJnxWl4KDXHJpmbldP5YZ%2BjEP5mZwioyL%2BU8bizSPEBUBaofdQZoMGpaveZ3fzJNnZzG%2FwPjqeA464q5pWgMHCrc8n1O6P8x6MIHcvkKCeD%2FZ6GHmHgRvQMRL0P43gQfGsKwJw3quKSE0CF%2Fi5qCmv5ye3rYGlEXqamAQg%2BH5lLwe1Ic9coU2NlrW2qh%2BCJ5V%2BWyGfQ41BJB2c3qoMZbr8yjXjC79ZDHBjqkAX0nNrM6ufb3bc6BOII%2BMwCx%2BO5AQ1jivdh8Jas8GvW%2BfWHtmLcfjLjJG4WeSY5DGkkVOre8XD3gPG6ufdu59n886CzfglTaZ4nKO%2BcFrZvfhhe%2Bm5HnOXMBYMXUBiWirV6fvJ55688EvISw9qGAMtccbvpm2khcmviEt%2B6J3JEMq0l6PWs4iUCZbo6ee7qI4DSRWEus%2FlI1lyKTNKqhuYNj2dMX&X-Amz-Signature=b5ba0afe45657d8b8f8419a6d8d8a730d45c375f739df196b4c368015286f734&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

