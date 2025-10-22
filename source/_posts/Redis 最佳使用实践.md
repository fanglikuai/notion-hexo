---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPJLOQQM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIEmyRUKDV3N5ailaaQw7N6gqo6AplnHKSgZRSep63%2FTvAiEA30R3rUr4cU6Tm6WqXq1NE1AwYswlrz%2BpY%2BfZhoSqqB0q%2FwMILxAAGgw2Mzc0MjMxODM4MDUiDN5MENrXFX%2FcQgjSXircA8H%2BogMw7Jf91b7%2Bph5SOGdxNSXRKmNMHEW66HDo3zJe4vfJY9EhL4W6xjJ0NsukPtH5x9Gpc1JhqXIeWec3fL6odhk37MqsAQPun99O8IQFfa7RqxeZ99irRqKbn1YhRVDUaoS2csOSY42qea%2BhFAnPCH%2BiEvpxQcIWVU%2FNLMsGIY%2BmaRllBG5bBnxWJUl0cZtjPiVR7rxi3yzFYayPuu34CGJfeQF1tt%2BqJ7E3HGtEj6vKMcSyxFuZfphhfaoW0%2BgS7IEcA9Y4YYgf6zTDOOW8ftWhAwYmHUp4EUte6HkPwd6MVFrXmpIov%2Bu5pcuEyl79V75gruz5cjwBmB9QBMEHNCjMYBQtZv1Qk3EFf2Pnj3Y49cD1Xr6Ts9zlcSBWvbl8khMJ1YdGHN2z2cc%2FicVkMglnOlDG9MyqVlcvJrLLecqcgBZMsDuCqqD9PZ%2FhSfu6RmP3uRGgVUcO9Y%2FMvEAfR0pqlidfEzqu6qKCoQRxp505LZOYD%2Fpd8e6OQna8rK6QmCUD3wJPYidcQHraxoMEanzzOQoeEYYiFvAyu85mk7qU2PV2CQhiVQfqcHSi0to37PuGYk6bABn4Zd9Xqg7o4js86ivGw7yBd%2Bh8DC745Ag9FyV9Y9JVOhfyMIrK48cGOqUBZgs2XLVO8vuoe54mGjEqDd%2Bomeih6x5Qhjf%2BkwfLbtPZecwjF5%2FqqVXJQ%2Bl%2BQ%2B7XyBKdy38xK2yJfKLK2zAkHihhnhVp0flQl2RI9k7srvIjrIrS%2BlL%2BzYzEx6xdGczEyESjoc4kTvb22XbMcrX1zZlCN65prNUkW2ncimnNQ5iM6KrrSnvzNFSmlEIQoz%2FA3xdKp6ubFfV6kmmut8mNT4WuxfKx&X-Amz-Signature=6aecfbe620cba5ec42f2793f4306ee2a16aab38ba9122c2c1bb4d1233023524d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

