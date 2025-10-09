---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOHMJBTI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T010113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDtTEJPjtEfkfLTTvjDmxE7zrgArMcxur0U7jU4xoD%2BxgIhAIoGEF7e87gSrWzmaSbl%2FGJDPDxDScWZoglutRmHWyhNKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwlLtbu%2B1VQxMO0D08q3AOR4g1OJ1KyVN%2Fd6Dv%2BlwDixGLeRy37egNLvBi0Mo%2FqyJmlIvR9q5o8Epe2dN58OrcvbGRUlZmTeVNvyzyGMz6s7Kt8JYtOp2PbV2zJLAX0yamMGLaVNuGs2X3fa%2FUzFhUuJ%2BStCQ7GBN14Xr0ThC8cVfne7DJJxDJdc6w7LvDy3vZk8YcFUhKPSiUtp5qu8vbCojdMylejGOzk37A38jemG%2FsCepOil%2Ftm%2FjtZ45kXDfHpFuNClatQtk3k1NAGoDNJh6igfnufPu%2F93r1uh%2F8yqb6OyECMHxz5OmaZQg9zL2HcO2rZHWRzFxRlgOn6Rgot6Kk3Pn5XcSCM6hRnacFdZNsKokdwj3xIUVKBXZCOykJ51QJILcx8naFyzFRFt%2F%2F00lZ0hZJst%2FBW%2FriOC2SJoMJV9HWUNsk72DnjQeOswKTzwaykgBgQDCDBGeg0y5EJ1AG%2BENTxVn43NtEdzAOgpjUbMqKTbftXAn%2BEuU%2FKdtNww46Qm%2Fo4vD18GL2geCu0Lp6A4hYqxFObwLcQIKAf4XqFyQ1JBteNpQwV12ZrCIu6HUqweeWeevhTS98cYP3typDTq8gRLT5vrWtNsTakWHmMGMut0EQwYc6SqFsCO%2FOrl%2FU3i7gC7mem9DDMhpzHBjqkAa%2FR%2F19%2FKInWoXERzJ5mAkk9EDJR7tsnbtgMQOKTX4y%2FSqAEUz3gG%2FE%2BiZkn2663OleUdcNdnHV6C0%2BJ0MFIK%2B8xaNS35faBhaR8H%2FOvNPLqJRLAffZT6eDzPpgbh0uGzh1llxfB9phJpFt2FZydhijD2YbMXGVJHX5FPc%2BrvXzQYHASjlZOXXJWSm0Rz5vGPHMo31%2BGkdtpMGWsumUYlWxiOKd2&X-Amz-Signature=6a59003d08a50e28c23c68ecff3bd44fa2d0500152ae2c4c6c0a3eb576e0ff0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

