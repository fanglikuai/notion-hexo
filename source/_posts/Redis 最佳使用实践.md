---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MDCQH6J%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCICmPBlh33mEHWE8B82%2FttqIcrHjFajUFUWWl7xTYRghwAiEAnAZwSoi0vbhRgaL4%2BNrGakY%2B0HKUv701Y3huhyNLLKwqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH8uRSjcywj6yDPGvircA3ZMHfxVky5CU1QuQGx5Wk6H4jRwSLsPE7FmjOEXjuqzUvKQ0NRtNjXylqwiHRpS2Xfl8eAQZjAVhDserpMMsXdzECss7mXNNhb0qkhxo7hEajYGbL3RpScDjf%2FI7o8PLst5CWdyKwllZ8sJhU1kiqMwVK%2Bq4PPL%2Bk7r%2F3d8YWkhEi4TTq8ezZ6CFi1zKZvH4XAhho2rGmm9e53pNijadQiSfQ%2BdStCqT8ODaQU8FClnfPneZSC89ALSWvnsnQ%2Be2TQKfqoMl0NnOxMP74SFoA0lffkkSnZ4DYa3oWX2JUx0%2FDBvRBbTwq1fAg78iLH5BaWmiUPuCR96n%2Ft0eFBZE3Np3nL1X6EWLdhx0t5W%2B2cF8KufGdTP4oBJ%2BX4vkwu7kGBx8Vam1GJq3%2BlkukClczt7MveGE4bEO6QsHH5symLtwC5y6StbV09imp2zCymkG6BZvlpFVB%2BiIJPcXaTs6EV49Kvqg%2BMjSDfhWs03%2FwR3RBuK%2FUZXRHksGGDhP%2B3A9L6g1tYwCxMUgGqKIPx6BPR8mAjFKVsTtCRx0n9UVZyr83VCXbiJ%2BbQIiFcQImnD%2FSTQZ4vQjl5FGQzD29s0FUL%2BLJEBb79%2BwHwMHIbWmaGAVUOm6nBvqCf2vQHoMJilucYGOqUBPBEkFuhxdzK%2FONDXY%2BdRZMOtoa46ES839XOW6dtR%2Ftld6yTbQIbmZWrSpLpxxRvO2TrUKdKnq6PmSfIkOgvYvPIY55RSJUdARAIjDOWSYiGK7dLf12AtpsBxgS6LVdyReSLSiNxikI5e%2BJnwm8Xa7GpmCT1Oo19SQBxcQNzJ72ckHW%2FOZp4IzmZ6YiHFv8SJXXeiotoW0NqTXVtkr4LXg09DyFYq&X-Amz-Signature=4c2e032667a1beee5ced7f5ee23f60748d2110205f726bd7ad0b6e12582dad2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

