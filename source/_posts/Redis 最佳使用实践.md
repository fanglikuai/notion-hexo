---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665B2KH5BW%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T110052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIDqFulw5y35m%2BKoJsQO7CHNKhtloZy41LRMeBs1jqxGRAiEAjpuPEssHCBs3TsJKDJ3odPrkdZXaBr9RzhPsYN4L9osqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOndRjt%2BcSOT%2F3VMsCrcA1Mro5cs1OPb06%2BUkRZk1jyeBzcs0dcr83S5cr5CeHhzMUxqrsLWv9Ro5VFzfrFRnIzYWa5lKgeJ7uwRSqoBbAD7nznlOUhOSWk06kmKInmZtOW0hQs5fjaIhdltP%2FOXFPRVRK72c8wgXf82xtuIj5T2VqFBtX8Om2vNG7vKC11j6Lmsro2ZTs9NV2AvNpDyDrHeNThstS1M%2FnWvBS0FIc%2FVkqinFKpljtwnoUkmc5G%2Bk7qvZLr3r5WVax99QkU0NR2eVz4FNetW63f5gqU09HReHa7SCCiEN35GNUJ1aRiOIV5BZGmcBLdQcvUutwJBD3qfmQ4njaO7YdLNzY%2F4KriL3j4KfqYGV8X7%2BPCCWZUSOScO27ONP1xLUvR3Cr0Ee3GdG7xVNQBZhVONJioIR1chXHqoJWdY3D95b6NGIHEekEjvJxyXb4zuU1wtXP3vXPumToQVJBBIrsnPcNVkh1LMk80UFw0jcpG5kZbx5PgUBfNPiAxNOGMlwMEKu9uQRcVl9T%2F%2F%2BHnRKV1eL4%2FRWVeZyPX%2BvEuNCvA4weF9QwWzojnBO7aDnQzq0hIbhF3OG29LPYF%2FrQFiyiXO8YpXISSepcmE2szzylrPn3LiRnaFY7DGSrDZSkDclBaGMNKRqsYGOqUBKqbM6qlx1jMxZytB%2F6Z0GcmkdfKzH8S9j%2BbegW3JqtRuhM34Sh3prWfFA5YB7j3qLSvncoMz6tojTEs8TAOsBN%2BJ%2B4XMOwKGwF1v%2F%2FF%2FjyDWYFrg7PojRnIa1TSo8AhjB%2Bq39IcyS%2FkTKjIoUG9lCRoYiZb6RpOOiTwBx5uziVtqY5tRoiLf2jmNS1Jr83OvFf%2B%2BiDpklSzE2XrGRGR4AwHN24hc&X-Amz-Signature=9b2f6d964fd779f124ef2ea3c40a5ab6ec4458011064df8818675a6034443794&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

