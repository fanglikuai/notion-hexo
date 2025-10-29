---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF7CMSC5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIEXGsgrS7U3j53QIHY0FXHR6oUTYBedrbZxTAnWat9MPAiEAjPfDzCARxtX2WFVL3IfR6wVCvacPLRyHmyKO2BYrGg0qiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5BT8tIHjXFQRiXYyrcA6VPgBeOab2rGPQ9KsqqVH4ilmYNNJ8ZOWNmLpWEdAeQr2MDZlPdoA5zaWpTOHjrBJmC5ZaCHdHCdAqM%2Fl134AAqAQeOMFuaeNiu8hXJ1IXh9IbQa6eG22guzfNZnDwnodyT0Qcd9QqNhTZIqBfrcIEDxZyRvo7aCsdLN2ILOFwsf0oo3SydYNsFqBZ5xDxVF3vDA49%2BnXd1y7P71VeDw5X95PAUgQJjlVDgmyWFfwMiXc38D08A0RPGTIRExV0wyKaS9dcg682lh25EgLqAIbsFIWPV3lSuYRHs%2FXRRLepKRNXBEuj3ISd02W9Dsc68%2FS60bzdBqhYSX4tcHbxHESw9dP5oMjp%2BZwTIpJJJYowW21Q9C%2BtM%2F3SUr2SLp7e%2Fip%2FLiJaeTjkH%2B6BoI%2F7zHJPegQYcgeIvwwLPrf7nMh2S6GByDz4zusNcva8sCn3e%2BSiVamzHMQsBgMBElwGeAGzGThnu3g6019voC3WtqYGNIm2kOXC6rZ%2Byip8qQNGcd%2FiIcN1%2BkMCLF%2BS9JmBgGhfhENBxn3AUJrwFSLZJYksAlwoFBnIOnhLJRbl%2BMYBhCA%2BouAj1onZSbRvd8on93u3Sus71%2F63VVdKsiwoGc0IbpyofmikBfMeEUDQRMPPTiMgGOqUBj8ca4kbdElawkmO%2Ff0eyCSRiL5eawlAJvejUxEWW6nwREiYgWSu4fazCmxbFWBPxRGUKT9yyPuXdThuauRTGS4JM3zH6mTsBANj%2B2A7RqNV2zCDb5r1Q2mvC66eZPR%2B60hFJbJioIbgaglgqaYSvp6Uf6l3EKPkmDnwOVy0la7QdkcAOIPoSn%2F%2F%2BlY7MonU9lie8nzmFNLjeIBsXwKWqGmA0snbL&X-Amz-Signature=6b4af804f9b0093b13de83c0440b73073f1c54ca7543eb3779d9ccf87a267895&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

