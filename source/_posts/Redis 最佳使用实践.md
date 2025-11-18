---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJLUSFXK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDN2%2FAOcqEQPRzCICGu%2FR5PqQpMHtsBkL1EABKC8QLECAiARywvBSajorq1dPdsi9GEPGsQIvwfrARrI7Jq4hmBlPyqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQARf5FdNZXY%2Fi1LrKtwDjIk%2B5TZZNoaaTcpHMc8YF%2FQoQ9ZhIGdovr12w1BJZXajLXdFftYd%2B%2Fic1cpv4KtTKs0SeohzJBtWcAQSGdETDSaglUVbJl%2BRcYMHwTa2%2FCC7bJqsqVe6G0CNaq88ZOxOsbK7ti4tUXncTB0bK7b8Fn%2FUJ504ZoS54JT5aC17woxE6iYPWjCCETC0%2FHOFUH5eHawwhVg%2Bbu4pKhoFr1BgeEiJdCT8v9E%2Bq%2B1yFM%2Bj2RgXHE56EMPcRvPhELocgWiB91dNikjG%2FE4B4oKHqW3btA5BcN%2B%2BJ9e35g8nVopUnX%2B5kpA%2B6%2BN%2FJcAF4whJI9TozNSxZ9TS0%2Box%2FRivcvQPJhNNpojDa7Z%2BwgOQ%2Bs%2Fc%2FCMQfcFig9E5Ar5J4Tlm5AyZC2S5KvkW51Xly%2BGu5Pc9swY3dqDsN9558%2FYdq5SdNk89gqV%2Bk%2BxUjq9hx43Y6SMmlgwWAoyw%2FbhVQDppMAOsqk4MaY5EwlAxgSUujJ988o%2Fjw%2FHkVIAjtTWl7M%2FwPrNOvDL7MQSzYnIQQmEuPb080dsbyUVOWzcTncVrDiaROpEi34jTo7T5mVX7qxaFzIu%2FyDfvWHx2ll2GPJLx9aOM8fq3JrngTZrgXCXZvmCM2IHK2PvlyfSW%2FZngdAEwqL%2FvyAY6pgHQGupMk400mlVTI12N6XTuBpI3CdiNCM6k82yCk5z%2BWErHT2iEdwGkzx1rBkuqzynbrLKmRzgm76%2Fdf%2FUXnN7A3wN7cRkeZyNVXthcboQZ5EDnLu7L2lsIJmDxmOHM5CKLKmOt13H1pCweFjG4S6dGt6eDc8u9R3jCwx4h7mbRTGacZSXDXwOESLd0UJz1GPWxrRm9dmgw78SsFdkQd7GcUVu2qGVp&X-Amz-Signature=a5e9df699b3cb64fcd91590c022c073b90520270ce8e75ad5640b79c86185663&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

