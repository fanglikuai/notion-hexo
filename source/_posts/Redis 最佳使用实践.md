---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MBH7YJJ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCICwHABGq3i6hmBqm2tICfZ5yR%2B8t0ksi3IbG3NTAsCPDAiEA%2FcUBQviMVyZMy2qbpolN1AQzLuhzg8y6brpVR%2FC%2FmB4q%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDNU08iELQviy%2FMOpJCrcA3MbdbKEaYrdYPeBcEffa5RwNgulC8FWsFEvDjuyKehopSEaOLrWPnZKzljlXwhLXAVUFIw4s63ZEugOt1F7zhd4yUCDG4b5uLqxvsIyxmtGjS2RsnFnjC%2B%2BcGNhz1Z93170SA4zRJmiQSi7xgGghoDfVuDftI%2FqwC1D8Lyxw9A4RqpifIxB7OLVbrh0oLtRAYC9Me0FAry1Mtb1QI3RghSlnD4x2LdcJzXvPX8aX4Ks7gkqeOxdNWGPOsfJbJWZ3LQh%2Bh7IH36yLsjYtAHdvcPVx0ZKTawO6N9cr9cRvNq1FqgVJ27heQI2MfIvsHynb4w%2BaYO3HK9108M5a9mPdQd59AqDA%2FRDLtnttF50ymV1%2Bzm498%2FlGAdu9pRS3L6lQu%2B7WfI3IqMU6bUK8zfO1SkDvoFjwGqXqPAb3ILj9nAU2xQEM90MsQASHcRC3NNnzDhabjdL9u58rHQEtRUAgYDYx2sqeSQ4WEnjXM1GYrevUIzm3wcTMbhUadDqvQIbfNBLhCKuucMI3etbWiSk9Quk7uwJDbI4h5gQGyirA3N21QQklZXZf2tjUDNCIbX3Fb%2Brk%2BFmoJsLA7F9ZMiug8dUNfEAfGGOBCj1s0rvk6XrpLiUeLyN8xi3F8NdMN7PlsgGOqUB8oSxnePqMhjXuntoZa0moa8vQiBR54sMzJOQ0qZpXiE05i3m%2BLvJKO54PFLMmKBvNXUnXh8x0vKJhUcGwcBcYateCN4yAxHdr0bHjRDgkGfBIyQEj%2BkQ6u%2F8fQcLwSdmgzD4XPEViEROh3RRRtIZwk7Ky%2B%2Fa2s2Pn1Labh56c6mfkXYqfXjZ40oy5icKSaPr3zFztYX5X81uAKVX5AdZXlzU5bsl&X-Amz-Signature=046406c01bad87c2fd61ee16b3d0ea181530a3356d092104015ee123920af56d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

