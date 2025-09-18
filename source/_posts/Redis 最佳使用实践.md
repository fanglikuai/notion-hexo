---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WYFWDE7%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDYkXq8zpBvPCnXoHImTc%2FbPcZWLg3qdPW8BMyH%2BwQXMgIhAKjZd5fZXb1ohOHIUTBAjNsTPK2RBn2mXdFETyWsE2M9KogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyQgiwQt4xE4ZG%2B1jIq3AOJ6FtyuKzEUqaPyHMLNDofGdERUiqKrshfTWrIOLG%2FD6b1H48uwRMu2WKL1zkOY3NyMto7zTnPCIDODb65ZEbcexILioqc%2Bt3Xke35WCazNmF7W%2FiBV2%2FHCDdd%2Fx9KPcRJ6yMUr%2FN2I5tU6au%2FcERN%2FDEJvuKmXN1UYa7t4zUxjQBP60XjndRxsfztqHA5O4TwDB9uMg9RqlJl5Vu2GKoxhwSMAoHwmf3AQWwTYKKnQThJ8RZ4KQd20XGpu08OtY%2FP3l5hOkCdhMOkoIfi82Jc6%2FIdfkg94JAeZsxfUoJukYooF8XYZlKelPWvEupBGHawbeBFEfL%2B%2BWn4ORFyRpQN9i0G3seR8tGlXJ6U2C5nrtHnZOxf08oDxcCBOiAnyT78%2FpvWorcJvhV%2BaBpPm8QUa24Nyyq9ghXHNMUn906iVJgKOQjWjkfBQnPgCVyp6980FPHt5IDwkqRHp04m8rxhRUCZb2CxppZ%2BTjzjvLh5Oozrzb9o2J0EqqY%2FL8CTQN0g266v35lkcsYom7aqfQMLyPKC5G5q2tWNANDiqvL7XA89nHFYAjnZv4pMg%2B3VhwUtHNC5%2BNTkbhW88saVeVqnF1AmzZ2SNCV9z7Vus9marU%2B0HNOqFtAzMMfVnDD4ma3GBjqkATx1duuoGEkjSWu%2F9AAghAtTUxF79xqgR7OlXLtx%2F4Z8Y2iTeYqQch0TIQaQakgYNXm6%2F86rFTOWrjpChOvnHGz49ZeynruI5nATeOsdRm5GY2xZId6S0p0yLoszs%2BUDWe5fMefZ6AruuiYGML0u3SQVdCMi9sOKsXx%2F1xBee3ts%2FhhqHLR0JF9ApwZ%2FC%2FZR316Asf%2FATVg5e%2F6mgPVXJqffTSFu&X-Amz-Signature=98f49b0c12af4240ff53d5fe79d4db846d5dc5316c3bd0a267bdafd70870474a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

