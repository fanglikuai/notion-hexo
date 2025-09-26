---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RE6BVMXV%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T080150Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQCPACzokJfpANSscZoTfEhDXHVCyEepbWv6wKeVVX%2FwLgIgRX%2Fdh50y65yNpvdqViH9c98lx3tXpOcUwLAISTkqZFMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMKdavrYSUZRh8sXNircAwOtT3z4rrgHYY35sfU9b7Z4a9Ej7REZGuDRJbnwYyUOWtyiKsYY6XPFM5nk4kK8YwFq4Q9le7mdtJ1Qc3x0RV5Pvfk1%2Fja8N8JMuEIAEuxv8Sv8VypP8bPkKLellBlL3r9DOwy3pZwlIum5VZqV9O2zOOKysdFt1WQ5kcBS5ZRu0dUOujjKX%2BQCGr%2BiQyMqloigJNDFz824IqFdERHCqvCUg7a9GieINOGU6DagFbTSAr8U%2FhKk4CHQaIbv8hMTUvNbHNgoM5URf6glzi4QUBU%2Bzz1hSIDOHHqigTluNIgqjZva9MjR3V8%2Fn1RSQKOY7ZHfnetGd9jdG5EOQOVStqlLfGrKZbjPyetyGnadi611fWLxoEb1FXx4PMNDUCvCEAv0xRjoshES%2FN7LWMmUo6%2FB%2Bc%2BrbrkVHejQTlfVdnl%2B%2BWcKEZsNdZXxQPLA%2FVKW8V04brvOB1lYEGS1SsiR4gelldHFEx8LoWqWMhFzWAVYlnvTNVCJ41Wlv688OLS2aJr4%2B6INqP45liSwWqwzMSnefzSAdHA10hHtMX1WMUE%2FB4l%2BypgyoT3jBN08ImWe1Jpi4rtA5lIo5S5NA0wIZWTo88oSMH%2FGby1f99wYUfGGC5zT0jY9KXemIBfCMOH%2B2MYGOqUBWVPplpV3fKeFv28H%2FFy7FxFeCU6EiREAMrmyoKSm4E46rW2jYZmX0pUo%2BujhYV2i90JVg6gf%2F7IFUm5UOX4tvxmD%2BqcNbEujdOas6m4sjNUwNl8ccN%2Br0spNC9h8ZB8PUZ8AoXwGVUssY1mlhmBs3%2B9CEEn7g0pGJs4aFBdWJZTAZFHyQo%2B1UWOnS39wrwjPRKWP0h0xqV01ACcVM08tbBHk5mbg&X-Amz-Signature=2861f1a78dedbe6400218c9a4cca4530924ec86b51ffa216bb3730b336059d56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

