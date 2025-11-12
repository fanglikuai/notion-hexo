---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DEXLHFS%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQDDSAU%2FwzFjxt5S%2BV%2FcexH21%2BYb41jq2Aokh6MT7pqpEAIhAPCII%2Fzktvn9pDp7RQInHGUve7kGBaim7HUFMdRTwvnUKv8DCCgQABoMNjM3NDIzMTgzODA1Igx%2F%2FWavk%2B27W3YsRqoq3AO9mP%2B%2B9vm7HGshMvsR3AHQD3B4xQHbl8thFdUyiuX16yqM3CEcnZGWFcabnZhVzXa4cU4H%2FHwrXpQ4wdKhjIB8a0oIdPMzxR1BnXLNpdD0%2BtA7ZlovodbjA4MPZcua7APUSxYiqRgVgWubZWvhM8V34lQjUm5XZlp%2B5vSkaDU2CVUtHukMXCf0SrqL1v7ELksSyt3nMOVKtCMyMf3twvJlsdw3V5pkpbhJEjrHnqHOd9xtp9a8nrEkZAcOKYY8yuTQCpkf2sN1weJk1UWavPBSIG4KkNch7FQRiUCFfK%2BsCb3hVDnFGwm9JbEKL9QrMhdA5QfagW9wUdHd9MHwRjHgy7TjeKHp8S77AweE9G9mnnsJh7y1Z%2FZ8V1kDB5%2BomfuZ7tZiSWjwITIgUVAiKp%2Bks4nsHKKi26QTWA3OBbDFNCutuIr1Z%2B92QPUO408YAy3uq8ckaoptYGDAomEUfnsxbyqXpx1JRu83xpbtelbqO9lzzikLYKMsTGg%2F6Y2xQGFgjQT3iqQn2AYknqnl8PaYvVoE%2BkHsyex%2BmkJPA%2FYdOEoC2UhxxWf8XiYieWEGjD2szky0nv0%2BC0pwBjAGTbxa%2BIYycttzNnEjkKDVmNd03572i2pwy7xY85jZ9jDsiM%2FIBjqkAYyUKK3fgRVy1qg6CixkIL564gfp4UsDmUff4fQA6JiiG6X07EgDBFqIjgdwe9M0YsO0zV8hflthbqZ%2FG8Z0b%2FUWGsdKGjqNLVMVOG3xErwaAig6FyXsJcnv7%2BWZh1sn5ylpqIu%2BUdhLL4P0RCD6FhPj1DN7ueaSRy5XhpVFlC3H4jSIHikGwqDq656BHlx2B7cgGEal72dHOLNfNcAIM2Y8JTK0&X-Amz-Signature=cc427c3dd320ae26ac67008226a9191e78e2bce18ff2ef543bf46ad96ac4c536&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

