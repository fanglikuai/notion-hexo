---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7JJKXOB%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIChwcraQVZ0ojuyvhSRwEiEoAdzMH4dBdy9P95J%2FmtN1AiEAsrLVHOYxFUid2m2r4deBydHTri4QsxQtOy%2B0rKeuSuYqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDClW9bVLGD6OSSh4WCrcA7wWFuW%2Bye448ODb2nFLqE%2FYoz%2Fs6x9IOvQVmuNeaOI%2F%2BDmE3oizXfGt5sycloXyCNBki6Ow5EfQhIdUZyEb4RGpz63KwljDHDc5jFiVsYT70G%2BEacQ3%2Bov5PQegWOKi5%2B8XXdVNrBSdCqZxFS5SzpwMnY0F4X1E5WWwktRUCJPvYBrq5b8XnwPLjXJVTiPFEIUc4fa1sZNJ42R0TLFXMj5RTVWwas4f3ijDGKWNWkqLUHUipnakpaHroyGwlCv89WcPGa52uBCA0hxQiNrClNQk5tNH9V1Viz%2BYp848LZ8oJHqKXFw3pRypuRsohYbRE6BPn3F2NaOr97V%2BdSZ%2BRcFm3W87WKxVBpuz%2BpnPmRkwOT0Y2G4mpe2QVhLm9MW1RCUlN4zllkse1%2FeEKyFDYtRHAEqDLrsuuv5lazaihhPG2Ylj5DmEmNWiJTH7RmAv3LgAUkVuq9gBiz3vAyTPDGgLOa1PStXizIBSYdMROYNwKoVxrFXyNkonLf25iNjqbpnKpCKiKbdap1O2Qshe7dVql8q9WDMqZjJa4q8cQIpQ3pipoHed8zrJttLhpayt%2BLVJIUWHJ8N7NiIB%2BRa304990Lw2usFw6y7HfMZudCiDp4LhKDhEx4pnMLePML35sMYGOqUBxE27i7lcSItPPnbV02YJdrC6UBzbMKvvR9SWKUEKonoPC1H%2FoapGKCbXW3iXkWwq6Fj6qum0btfqtF8mSvipHorxiDvxWKei2rWFZqRa1Y32nGhB6fNLDXPOMyOQsIs0zq4DpXBM6kL7J4D89lYRYhXV%2BUT%2BbIPmWzfInzhNNOxjJpkcM%2B07nZrhy1SUOsb22T1h%2FabwefboLWkEQ2ZmJQjhXzkQ&X-Amz-Signature=b9ea941f76658d86feeb6fcd3ef79138b409c71d5b23010ddebe0ce8d1b4fb8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

