---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJL43OOZ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCID8U3af0iJIuR3kk2VFg2grw%2B3i5LmBWIw89VN%2FZ1iWGAiBJGabjtkovF5FpX10dcSeBZ6Qkt4MgjdDMBKy6KWxRnCqIBAjG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNlUH3tByurJUzaf1KtwD5RupOFiYnfe%2BEIiJf3Nt2ZPTGIVBKWKi7uDyiuDaqAWVgR72ITpnUX8aaAw%2FcwTxv8F9ePvspAy7nUHWylhm%2B1q0faHChR5Lal5FvROy6hKj1S3x7k0wmIUP3YIuvMv2tno4dIKjEM40hDLA5xr0U5EgxmogHYK9uy3Ad%2FHuIPbjwbXODVUYrGybPKd%2FMwb5C0WL4oBFCHYHPubcDjisrggCnib0P6ASAe47B1Xva9BdAebLriSxcg1aNzxiwq4RJrZH2L1vAHvw%2FML6oW%2B5xZFSSKk%2BYiBHKu2BOA0EKtmV5ZJ%2Bq29xn5oYtEdlFOgprp4NEAB87jhNeGKuNzJiQdh%2FgUDNyRjAmiAyHW4yNNYfMExkxqJJu5wrDU5GxI4ahEc2fnMW6wbgiArdl98pg2bRRH5hvpJkmgtZnG6ygJe3xsRBwvh3svgwrZcqNXFf4r4xdhpTkgYu7byfbCOh2AR%2FHfk6aJ5n97xfbteqRYcp3pCmA09wRZ1rVeLakJqhpFhpAqup3zOPtc%2FgBMkpkHS4ZRNbmB0g%2BUXgSJo8byUxbITOyDhfY%2Fudzs7VdAeA2PtazchA1AkFSVsH3nVXzOZEh3PNiSMuza3fCyZzsU5pllTmk1BQyEtf9QAw%2FbnmxgY6pgG1tWIMsXuP80n%2BlIZlZBBycX2Z68RIbhrZA1WS4bgjHTucMK3J8REwFGG%2FKUWeFzaLmPJoySMzVt5QyrmNh426Bg593lWju84fx%2F2qdJYBL6FVvB8BLLb634hAnrL3R41uBnBT4NHb%2B8bB%2BR7SFipn7s%2FGKJ1R%2FU7UYJUZcK0CXRDipJAOG33g1%2FTJXceyLc9%2F58stWJJsj19ebHxO2t8XGHb6aQ0O&X-Amz-Signature=f32f639d42d75b0f8bb15024792fe2239c4ffce4eedc2abd737589ff36b32d9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

