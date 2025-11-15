---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666CHKNZF%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1udryz702oVhjMPeYiSXdl3RacXd%2B6oeuiPeckYORYAiEAy9OtTswi%2FEVbXum9qCGXfrlhoYzpdtPpQX295cpylDgq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDL9sOxklxSRalY6q%2BSrcA33%2B4Wzxiik9m0d5YO%2FQczEzTR1oaqs0LpLBDmAmvaCPjg8XIXVgDx%2FZIaZqx4Oor3gK2S93ZNs6iBJ4j4yPdl8Ou5rFKQPwSAMzL0SZSO7IwhsvRVse7tuVJPzQPlQ5PLIICb6JrmqXQCAasF5Uy%2FAtfD4hyl9LDgyiwVB5ZVl7U9Sb4UY3hfbuIMWwrvjr9PoHIZwFGeu4jB8J1sHereQaZvokWkGsLeRh21N%2BNXvyNf5amr42ft1uCzIpu2dwQb6T8waSP1aLgB4GPJriUVD5XrR10xq1jhZ9qNylghKLOJEYaY2DiiN9F7GJ6uwnlyEHZrtuMchQLfOkrK6yhE7As%2FbUSqE1zxaBQd7BILjrDZnwN8oIxY2ylFx4JMMubrlNiwVXfIEppRt2w3xiT9yEGXp3j7aUnBCqe1%2BmykUisJKDEvM1ZMcIgrFAVuRdrWLTsXI86t6iW9dxGRQcBKaTr346Ime0%2Fuzjjl48t4jEvMFtEdiWPU2RpOcecK%2BeB0vbCaoY415MHrWrbGYInB0ULvQSna9QmP8Y9Z38QlexDasjqOZTnA78fKcdOWIcwoBcZFfpD36B1zvDhbRb%2F8tr%2F0crrZ6JssLssvlDJE8LQhmdHzGRRmf9vXROMMKQ38gGOqUBn3aMSqHA3%2B7ZIoevfzkcoLfA7J3gBtwU%2BuyKeFZEzXfBe5MqPiYrtoTtjaa0%2FRD%2FxGCmipmEqApAa5WEZg%2BBdbjHrC%2BE%2FkMWCRVaWGikKbeM4%2BvimTQbTxaSeLFOMf1DOC34lXAqY259Swyr7sJrE34RXjU7g%2B1tQbA9lrSTIcm9piyjV8oe3TC6ByT1zs%2Bol3Ks1FEYPqVuBYWqNXKO7LdQnmKs&X-Amz-Signature=227cea8b76aad0aeebcd501c27998ce3de7ddeb1e2548b3670d15a20006a0012&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

