---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJATL76J%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T060036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIF7SN%2F%2FTOGhQrFhSPU6f%2F%2BV14kdegzCCVbqxCyq%2Fm02cAiEAzpNoXy4SVkybzLOUlplsNos4WipbAfVOYWa2KvfwapMqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEdFSuInq1rxyCJ1AyrcA0cAxKIATFsaQVKltVb1uLyObCzckGmgHBzv7VDSc8fdR8nEAJGrqDaUbKZQv7L88IjMulT%2BdHHiyyzEluI6xODQIPg%2FLmNTyQHcM9w2E8p7QMkUGt4L%2BihHs1QTmqSu1UXjV3AQoAspjVBtjBCaMmrfNSad32AYTU957A87Xv81%2FojgcY8zCUEQ5GQaxlE2DCi9YA0njGBIi0NVK62cmBOwZLffqQUVBT%2B3q8utxcWbqUnmIva99vQPvUxaUfGcmBKhM0M2vBan9D4%2BctsyreI4aK6jO%2FBSacGZLmhl4S8KnKrjgqEeAzwaSdMwzMDc7jexgopMOqXv%2FjvLmy%2B3NpdiwGQ%2F95S6qiw1keIDhSF8mxWCFQqcc9yhSSnEB0lNXO7UfAn0vHvI9KYrHLcyl04tnkdUc6WIfleuy6ekHcxWJ8NZKVsMvPypv70saQ9tr%2Faf7KP5g7cI1X%2B6B8%2FQd3hm6MvM2HFBC4IvMgAybdPLz4JOwubOtEEr9RQgmaTpemPNR5BMtxERwuA0zpJB6buFdqnQ6zo2nBxLphGXKNWYFMxu2n7YriYKRfzgSt81Kc7VSSUeRn4LkAX558xUjhGfQNh2%2FblDgzGh67HiTGl8E8JeTbZJgF%2Fc9JggMN7vi8gGOqUBOgm0NHsYsbju21RVrJHQRE23E6xFKLjO%2BxHts2rgL1KJash%2BmTqcpEAfnCVHdwbbaz75THxFFsH3xryGp4kWqaW1hwUd3X56t3NpzeHeAmSFXsqhbUglK6iwD1MnG3MQg5bacuIv7d51u0Gb8f6G6XnZXdTCaTtn9ODudCruMTunjvDYIRuQrYlB%2FB5tjEXUtsC%2BLV6ULQpCR5uXt8Q9LG%2Fyp1Qj&X-Amz-Signature=e0279e5beaaa1a5a92663523eb5a0d4cca1d97b3a1525c781820029f676c898a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

