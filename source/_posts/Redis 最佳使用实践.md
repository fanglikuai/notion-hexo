---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RG2BE5NY%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T030045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIEDz5g53aE3aFZMHZ5ox%2FCqppoN%2ByBIH7HuGaFWepUHdAiEAw4iUBBmaOBZlWwdyRErQoCED%2Bj2I966drJ2YA8w4ByYq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDKGo721q2pJjZxjHJircAw1Q5MCfLFKC7fpb8WjUNDE8zMrr%2FxASYkuLNF4bpdrBUOmoljCKZMHCcFHNmETy%2Ff45RtA4LglVpwzfRyybeZvQAeFUOUDh8WbH3LUm9dL%2F5S0GDmfOWv62yfGvRMmy8x3FZe%2FzrJfitrMWQDAUbAfMPK%2B9KR4cT%2FR2EI2itrQ7oPMI43rCpwfR6X068LMhf1gfYOPTsEVSunAE4u3ErCxkn%2FX8EhaGkfKh%2FDMrDkK2Ydek531OCJt%2FTqhbjGzhp3nQx%2FFCqRdaWw4Rm5iV6f0fovQiT3M4mKnkjO7eRe9NsplPbUOlL459tyNT0EZ7c8TFTgE7FTDEszJ%2Bc8n4%2FvgzQ7JwVbI83yPQRISZkjB4Kz%2FQpHwNDwmFWKvfAFRTinuCI92SM3ByIh675gr%2BM%2F1%2FhAe1yZCfNQu20Hfs%2F9Ly%2B1xllGg1SPgXVfnvoTKS6scPA5j1K4KLv1HmHJKD8w5SLaucKuMMLooX5ohbnXtcmDLigdA8dCdf6bc7wMYSHtjLY8rRuC%2BZ5v06G1eJFG5D%2FJOtoLXz8I81nq9lx%2FRn2Rb8uFcnj5yOwF%2BWqRjZGxnKVM%2BJ4B1NVGVbZWzXTKb4qpihColug9Bno10xlKiUTifzthVY9D59O76LMPOmq8cGOqUBMo195yrEZsbDcYh8nH7JBPusZ1Td1xDvM%2Fpjow24ZKobdj75H7WfFEGk2qIJc49m6n5MqARYc0%2B1nMj5haEAdEnhSEupU6jsz1LDXHguAxVcVpRPi5gXbRs4mLU2D9bnS097rEqQAC4Rr27ZVd28Ow2WvYP6hGWMhJCmdsJvWwolW%2FYM4AX%2FOruDKTmTXPIeG9Tt%2B2umQKbBhNNi1qk8secF7tw1&X-Amz-Signature=808e8631eb1ec71a006312212e934e5ba2ed5f7040688d95d06dd3af7bb677f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

