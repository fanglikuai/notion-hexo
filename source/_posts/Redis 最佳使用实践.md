---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673G7U4TX%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQDCOR665SU%2Bodwb2rfRAXtqX6VMMElkAVn8FNnxoFOjKAIhAOYSzJ1cskOcv3P%2FXTqLVgbfiH5dJ5shR74P6Q9o32oWKv8DCDMQABoMNjM3NDIzMTgzODA1Igzg2kLjh6j9tteR1nIq3APIplJwD%2FzdgP6POzlLKL%2FSRryqGuU2KzYm4Hd1Trag76pvjb2O7uA%2FgTpNauNbrS%2FK1riBiHwJxNKXp8PrP9zqfybsgg1mo28%2B2B5DoxWQP8adg81fdCIndb6vNh2p6FqcGTsKtLpV5oXyIkfaWJUBX8JnUjho6CBNSmW%2B7%2FHuisUWh4M2LyGoBE3PtbbOQ9WpLZNMKWMMJPYOS7q4g%2BoEAn8yYBT2Xn%2BsZk1v8ahrxNupHBkPAwuXQ%2FjkjYjc9v%2BBS5Kta86s4DIFGrhE%2Bo0bpSNP3RbgQez%2BkLsrVq6wzCeM1vDoDxGqYSmIBkha44ixUaUuBP19GVQmpzyEy0DYsS4KvJgejtkQoFQQ1B8ihsvmU%2FZHP3YwPspV9WmYGlXOF7TR9thwzu7buPSPhGK81QxZFDN8WiBGJAD13KEtBZZgYhGiPNJ6uDj93EDeU8dOv%2B1KSf6edLsQp002Xhl%2FcG0kmaMT5tMlRlsQhSkcQTS6VyV9NEMgNs1vnzilNU6gemoegQG1Y3ku3JjIud5Gp2fLZz5rhxztuvU0DAJsPx5EV1n60UNwLJ%2BOa2ItWGyk4Guol6NFLTQ535Q0hzPqXVZn0MqtfIB1D8GvyXSYsDBi9nNucwIXb76I7TC%2BsdHIBjqkAWyfhTdFapY9eOapyN4W%2Fzxr0nSy%2BKIgp7df%2B8PwWdnUnXipoipw3660YiXWbiB9RvUNlLvUVhOsPtI8Zo1FkAYktlB1M5fKUypGwQSj6wJlg9ePjbfCExDGPQEsxiCVXqAKDohe0FEsfK5kD9oEL21fWOZeoJscxMK71IAJfoNjnjgNQ2xfw9AbJXLDLrfsxqzFfcMOmfeHaQqF69Ylqj6RGUwC&X-Amz-Signature=a4e16f95f40c89b4446cbcf502b895702c0e98050705adea4ebeeb2893c9fe71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

