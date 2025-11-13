---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBQTHJHK%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIGv%2BiNdE66S5YIF4IKzI4NgKZe1yHOonfcDi6vSawSemAiAqQvontBG6b%2BKpxeGMlBNrucMyW4TEW26OkJenJ5S3rir%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIM%2BmToYV05dHcu8Y0BKtwD8wD%2FDZtUuQpD6mPtJ3Zj9Z3Nyyl7D2OW1fjYI0bNavuUWah5Y9gtqwDpiw41ION%2F9SVLFQhm0%2BEFqdTyk5dfQK6s7Zu6svNRGPbWGA%2F3S8Gj3Y7LghII6SFuKZfyHPBN3ZGNwGLm%2Fc7tzZ9OMQOsX3X%2BwtcXttLIvw8PoPNofun5xFSbXDSHBNEwwYnIzSvbFzpuouo2jrQVv%2Fc1bPKJrvdnxsRs9Fv8lYlT%2FGkMBvcDUzUAE8gfc6xKNfaSl9s1pqlLiScHdJV5lLpkP2hPo1OEdMfbOX6wACaiqImL%2BQO21iqHZJDbBHW3pTXAwtdDUvoTvCeNtDbnMULDOoWRYtH9eft1ccTGuN%2Bi0n4pKS6LqPcoU0Na4zqUrHt9j1RbizcoM%2B3v%2FFFVBbdiAJrj9rsJNs%2BnWMUPrCCys00xBAobEldjUKEqTH9JXPsPZYBL2W9RkD8TITGEnrxR%2Bk7jvZQeBMBsIhkwYILOHPs0vZ72JgmARyEQV3kmHfRbApfjEJAUu1j0grxfUCz2N9%2FKjW0bE6H8jhrWhm%2B6cU%2By6g4CLxFxCcGrx9lHiGDG4lJ7XOsw5PqhZXIED3VvwUXH%2Fm8tN9%2BgQF838EbH5x9MscMujLM%2F72Lwd5bc%2FzkwptnUyAY6pgE9HOoiLW36asjFK%2FyHEy%2FSSFXRUc%2F7U7XJxCIS9DH42zWCCo3iy%2BZZgd13a5l%2Flm2if33ypIPuGvEov1ib2XfJL2SwH7Dv8xk%2FU7RMK14c09Iaw0K81sY5pO52N2DwZX0ttpJhIokcq0uOvWf5G8MgZq%2BfwZVy2wPodx3BKsw00llLi%2Bd0ZBxrkAD1F7hgFGmianestmqRS0ZIboxS%2By7%2BPJv%2BUbJg&X-Amz-Signature=89e2de417810503e7c347312c6d8fb46aa621a9b6574dec80324c33b4c1c47fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

