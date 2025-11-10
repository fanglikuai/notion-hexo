---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNPCC74A%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIFpqWx1QMX%2FKxfPE8msVzzgfREExEQZ8QLE7Q8hM8DR8AiEA8FuAHjO7u1P0UML%2FgRWthNZ63lPm2n7W3xvC7LgTeYMq%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDATW8e2Xj59h7zk4nircA2Yub6%2FDeww1MvwRAxxJvGRF%2F6hD8aR%2F9qBPYxyl%2FxAv2cHgYiubGXy1mfLG5xq97lafqC88KSfmI3ailg%2FJ2cjCzJoEEp3BT8bjb6SF2WU6eta5f7CL4cIu8OPxTEU59Tp9wgWe8Wv%2B8%2FFufLAiVq2L%2FZkFQ%2B3R963kpnCamJGj01%2F6xb8sZwH8EqnH5MRGgB93uAmyCmjUeVAfBUrxJShUQRTgl1qi7z5LXY52Z5upM08zG9qN8C%2F8k32d6fQiTToSrj07bbkOosp4j3jbL88rRMF9WNlOUVRoCSnuJAc1df2LXy9R0fiLAzmtb%2B33SdO%2FQSqsNFCEFnckw%2Bsb49us9TYH0QXC4EKmimKAXeBkYmXXk8gd3cX9oHwc6rl3unGvd0lFnPlCzGQBFaDIFeiZJLPUaP7AjZvDo46Ody2p6RP9q6S%2FDs7J5yIRyT8%2FYZQdCG4VPZrMj%2Fh1i3sdI9mcWHFkRv%2BHZbr%2FURJ%2BdAcfVQvtj6IbfEVHgf1fgCrV8CaNo9pE8ysaw%2FT0%2FVyU5aPZpCEay3ARs612itmwruM71gSOViejV0AqlhwW9CN68A8iK5DR7erzPYAe2u0jR7MBChfR1rAmreAl9mNsV%2BQwpgvqguA3Li6YncGHMP2YxsgGOqUBareyLUKrY1JR2%2BKL5%2FEbSnNGsnma8DKl3%2FHX1wR6471epWpQmxDLDa5v68YxDXZmeIs0S0Q%2Fl5Gewn8wkLh2JqD9mvWn0YXzfBSZl5%2FSi%2BX5YF9l6XwA9B8hOE%2FQDkE9nj5j1Nun5S9L5C03aPffPmwAzX%2FG5NBKX8lKqWz80HDV%2B3HFKzC19aGA5RLG3Gk%2Fd9g51w8Zwl2LwIz8Ju3W5YbyDo3o&X-Amz-Signature=78de5c27c8a7c15e53a3fc2ec8c1b85457cd01cb112d51bff31c2df324fdda0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

