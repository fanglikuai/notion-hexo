---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXW4EM4X%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWNcDPAAUy3sXWCUctkM0h56NHEIH2LCqm28r3C9MSuAiAPfJZJFNFt0wgEZ38w0QjL2TRn8MWxW8kW4D76tQL8lSr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMHrqFdpj1W8%2FQpES5KtwDThj5Oy6w5KIQwT7zktTWVEoRYOSNXPiKJ1Ziwp4L8XOpAoezVgGiWeNqwEukNIo2FzuU9IQ23eWUhKZ8FuibrVDvaUZtXEfR%2FmKPU9Y7Y1R9j6XgWO4qTzD8Dd6X3AGijumAVpF6hPhChanAlIfU86KGEf%2FCixtfpXUstd%2FBd7%2Fcj3ze5HxK561pd4M2pIwXo40LnXYX9Glero%2FZNLDRkEaz%2F%2FZh0d%2B5lHcUgr0wZIs2DbboJkwqRU8uMhMhGPicguDe3ypgMQHIbc9JgLPQwbfLTrVz1O7zLYezItCLR5QhlcPksk8ky3vyM3OH0PIEDt7pm5Uik48FePyPURiI3xmjXaiZG4peIbUjVCXCl%2FqQIO2Vt4dl2cUBfAvM88qC5wyQulx1bYgDrHiUnHHfgnSZ%2BHdWr7v1071%2Bksg8xDfS3si5kYFwIgBz9uviB3ujYLIz3L%2Ff10V%2Fjr9jCvV3M7LVxjp%2BeCpG3KDrXXI16f9SkiIO9kTK0y%2Fr%2FDFOdv1LMHYfiJCVUE0bNGGPv3NevOHvbAlJf6paK8kMxFzjy7PaEsOmw0omlcCSvm7niSk9I0SUsJDPLuLC07xhcdEvX07hNtIt4AZKByexCJYh9Ddt0Hau7VL0uXAWBCown5rtxwY6pgGA6e6quDo56gpyGvFZpbX5X47f7Fz9DKHhWIol0K7PgHaREIRSM81M%2FuFf%2BBLuXxnSagMKGxAa9luoGeNXzclirCDt0HqTu8iOljwqDv5MV5LWUACr21tDwsAyKvqdpJRoEH8R2uko9XMp69kyblTeHjjFbLR3o7VYyCXcK0kdDDW5br9cdkYi59Ft9WaLVy1Z7sa%2FeXKnRVguoJYAOgXLXz9pr1BZ&X-Amz-Signature=d73537f0e518ba5fcc44bec74473691908f694fe3913ffdaffe586ad28f46007&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

