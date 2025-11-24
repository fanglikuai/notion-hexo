---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTMVQHCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqLWT7hdRtmSckIphqcavrS7Pc8aoEylypduG2AW7MJgIgTSqYBVmNnk0emdd20yGN4T4X6NLEGAkcx4MuMBv1Zpwq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDLjhhSng0qfcib4tYyrcAwlqfSz4uvEHGEa0AwcCrJhNEBjuP%2FvAlFjZv5Zhm32k2mVtJmvcnvyRVJW8FbeQ%2BkwBNoi6dhtu6U%2FV010NReJ1cacpwEGIX9IJyuJCOfzReIYpg0wuAmZ5nqt65IItIih0%2FBGz8WP8wb0BY8sRfTFbO4Sh040N1w3X2P2%2F9n7%2F%2FyEwSbrLv6108CREaHMHrvoWQ6iU3LU7THkyjw%2B6xNBXlo0UZvMjQbk2czXTYVAI4Dd3gaO7NKAKeLxYl1uAxypDoShY1%2BuQgBMHcxAgKc0lAEKMU%2B6gGx%2BDONsgxt9jRw1LPdQLkk1hziZfIHGNvosCB0E7fjO7V0BZdc0NLxNC5VFNKW4NTf48X31BGPJpienNBF%2FXI%2FppFNiGAXyleofWMxcwjLFd2V0DV29qF68IGLGu30jZsQWYCZzWVcnMS8EMIzzoeFr7IgrBG%2BYqi9iWuSxLVuCJnggy7ashg4wSbStzwXghGcwCcUPTWibkEekcPR6LKuBlaVz7Gg7HL%2F9yiy4KKXiEA0d%2FM%2B9mNcaQAjGwSbYwvztnRp8JtWQxK6N8GNR6GYLAJpjucwHsEqALvdKMJTXcl659rRvwqdtfrK97494Ss4zo01DgkkRTPR4Y85VGtLwc8UVKMOXkj8kGOqUBl4BjCb8LKGp3%2Bp1v5PcREZ3%2B60gKtriR%2Bu4k5vzpQfjCt%2Bo4ioz1Fhiryo8VafMQ%2Fh9FA9XfadCQfq7wW09sd3pVsK68ICzS8vAH9xpwFT5f3xocQbRkh2IKfAS3Gw2sWMWGedq08myEG9Nh7SjjSDI3tebwm%2BHH9cBiOSvhHqwOqBEF84fp8hHRyxKu9EILubBCYHjRhh7UIOikPX8ZB4MRTbjr&X-Amz-Signature=d81f4400c15c98ff418e3ffbcd897751022e4972392109e0b94d1fae96080dad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

