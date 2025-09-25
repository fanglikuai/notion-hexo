---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y23NZMMS%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDtWTGz5Tgdj0DvhBWhwzHiVJQ2Awaqhqnj9JO%2FuShUmwIhAMsY1xeXucfz5%2B3GBkFZBw4SECbRoscJa4SRexAbeIlfKv8DCGgQABoMNjM3NDIzMTgzODA1IgxRgziVL3REewBAHLcq3AP3ZEMqWUF53ozGiNJGBuCuzEIcLBVGMsr01gvowtt%2BMOKYTG%2F9RATlP7108biTCYWiFujePCxBCXiYRvOF7BCp%2BWTuaf5sgm56zh%2B3UTruHdJMrGJRQPHsAcTT6iBek7iLYfKZGrtYE2NgLsjDebpvjgEUp4apF8BZUCgKcjVFa96gBryBpg2vpPqcQveqpP8wXhgpjkxHPoDZJ6UIkJ1A5MSwi9d8JcQqeS7r1FlEYMH3n0bzbuF15jJ4zslhkMt86zTa4MtPE8XN9qnk%2B%2FvhwYnLj7705LFqCrcFDXC05asz1ZuWpoR%2Fg8aPbTGA93ABxSnN4WG8Zi3L5jSHwLCwsa5%2Bu%2BogJGG38U%2BxN3Fz4lwVDc2W7lw5swVxm9vqu%2B8ypRyPsnZs0kxO0G2dWLwbJL8NI%2BSAL3qVDSo0KN6u12Y1In4uzl9azCZ%2Bhb%2BNcC0wMd4zfQU9zhb1CNLBV3j%2FbZUIgURafD7XIK8%2Fq%2BO8kQ16w5lyKd5mH8o8aImj6sj2I2jHkCwe9rJVyWzgkAP4AjjKd2an9qV4BNbq5cMDrrXV95nFJLzXkctKPk%2FnA7O8gF2gE6%2FnrLLkhCNXLRIbDhjlzopqrNYUdm%2BfaVq7mdTzMrRUfvyU8XyhWTDF6NHGBjqkAaRjGN2q%2FmY9wjB6YwNNJOyo0IzBaImRY3VDYzYEmhkkA7o96Q853mhX8VhypNpWDcVLFoQTrrWHFk7OcoiN8kJb0yBoN7HF4HNG9cddXQVpb5V1PJzukDzqYGY77IvcTVClwoPMhV%2BVWumOVgku4jQMtjyQkn4G6wIc3ytE3okVj4gDBuWgqcf4NCD%2FqD4iX0msm2N0kWTocTw1JD%2FlYr6leB7q&X-Amz-Signature=98ce051408a4f9c6d54756c3d876c9be4f1e3ca5d7c4a54667b951ffa55c6f9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

