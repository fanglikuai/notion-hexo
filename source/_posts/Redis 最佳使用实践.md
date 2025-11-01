---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCKJEAY5%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIHzjd7A7H0wO8S5Jgi0Q0Z6og3uvQKavfEWUnybZc1QsAiAQNIdZ53izto2kpI%2Fnxqh4CaX%2BvLFBcxocwJuu6DV%2Byir%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMuDm7JaII46DoJ54UKtwDOu4JA%2FlxDlSa1lvO5Cg6dcVrlRRYW0Xu5U9UFQ74CC8O9ezwg%2FTgbJCIZRcEP5hOg6LB7z6mPInVGkvkDpntujAZZCDvPzYUWQcjhOrG6izxtx7xpmBrT%2FlHFHpqZAkzl4KIg47lKhCUAXSXU4q%2FW5fqMj8HMvT1gvriE0gvFgqkNhqvSqbA3yJZbE8nze1enaOBFhOh8LjvF8Ne90zzUxwn6eOZtqdVCzhDKNXQ8LTtlOsDYw7Ioa3ZL0QtZv8BRN6EQR8n1moqHSstKhRVvRK4yPy1XRGtFlD399EIMZ5A6dQl4adIGgK7jYef74EYR%2Byivb8prbiKiEYDBt7sc67PRs5hoCwxI3YMNKRJ1cxM8aTFUBdHN4qzWbqqS%2B8hCiVvHfyB73nD7qky62lRw4DBs92EivO4lrwp31gw4NaiWRV9lD6OMSRLw4f509HcTrA2CgWA7aW%2Fm44QLMuwotI%2BQJ1clKjD6QrQowvXYK1qDSoWkGlq0wE%2Fqo%2B1%2Fljm7bZEu2i0bj1s8cl3JEtbatteqhwXrjybRdi08Vr86TQsLye5ivZ8kAYEF171tU3LqtmX%2FldRWx%2FblLlcHs%2BbzvdjCMRHePW9REgbe7qJ%2Bl2OhEddHxrSh4%2F6ZNkwoOiVyAY6pgERXxYmf%2Bz0yz7xJ1HNtJwleFZHnqg9sYUappCR6BLPRRFGPa5FKvJOViYOBpYVGv4WItrugYRLlWMqedktTcVS7hmfE7povpUTcYfrB%2Frneaip9aeVz498ZMWPA0CdTXLtQw74ctuWLlRZQno5kRS3hFaMqzEQxmym%2Fq041bdLkMf9oK3dONCqN51lbrvnBia2JQNnfPAhYmoK85E5396K%2B1tn%2Fbkk&X-Amz-Signature=c3368bad878425d0312c077af79a43c774527f03a12c78b41fd32cb5f8a18c99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

