---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2X4WAB2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGRO0xnB8xDHRZWbi%2FZ7gn6QQvPVyTsQ6ZIdzYHB4%2FKaAiACY5VQjmoEW2G8fMopoRacAgRloqsFU%2Bi7H4FP%2B1WOPiqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIME%2BD8OROHR%2Fh53VsZKtwD%2BVuzUckkxQQNFPRghx4JqkI3l1qnTMUqxD1upmXLypyhejQ3HaZBuYaX5HNTc0Cru0q1fyoDJxQisps69TBK9zYNenHYY1zt%2Bugoqvde78ZPyVTglTWm9fbv8yrgOhiTxC3dL8GnAj4KkrRLMyED0L14wOyFSYNroJC9qg%2FrmuIJzlTazO%2BUK%2FsaeDc6EETQA2YwG60%2FTFj2azxG%2FGFhHOkYZMEJMHsorLUAdQm7upHDn1ZfQm6WsvHsT3SaWzrGLPSjs7vTjtiKRJ3np4a40S%2Bev2W2FN6rQm2gt%2FvTG4g%2BXocXcH%2FKqyRZYZMo55UBmKs8Phyeki02XFBFRUh3PX8GJu9RZlEGk0UTuPmOPQ4MV0%2BBPDsMgrhbMinRMN2lleOz%2B2uOMD5zEFtLq4%2FWP8IT42upMPovyjiRY5HB17Xa%2BpMxjlhrITBmH8e3ysEldUQi7GnHPOLdgCAC7ynru6zd%2BLG3C4v8IcNyk5KgP6OS%2BZagvRcy1mkJCIoKFrYUdm2TW4e7Cl%2FBPBOT10kJN8XzDg%2BEEiuP6DkpTpKLL0o0vPco%2FNW%2FrGUwyb5LUt2p1hi6gkUqHMLmvsR0FHDFVHJw1CautRlV30zABWNJRexQ6i5PZpLYZPuGXZsw94yvyAY6pgGEBD1OdEG9AWzSGGQrIAjByHRMgOViBOv%2Bc%2B4HTa6PMoLcY8uiG1heN7gSvv2RkSelcLi9Q41LpOM4VbVwy6QhWA6Bzxgon4QkVyxWFiN5q%2BufqGStzyCPItsCFLfDZ8k6StjheTJiOpbQOyUB4XtDFuhVf5f8tzfo8WXDChOib13dCv1fupQXarAeDfNEhviIqkHXtKpwNbs83Dyk8RKMszXPJ47s&X-Amz-Signature=91fb73d213cdd5145036c1533f4d0b5812a642396de3d8906fa256cc1607fee1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

