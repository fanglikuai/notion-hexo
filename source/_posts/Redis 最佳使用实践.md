---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7SEWTE2%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQDDKS6nHb2JwmdIskAprk4X02ntLta7zQGH85iT%2FeFUBAIgDFaQh2JAdcr7t6u%2FINVARwMPM75g9oycYZkDQDSx2A4q%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDPnPJRK1bIKc4LLxRSrcA%2FS65nFjSSvcHhydE0h3TAwmhLbDlQ3i8umltgrmcRsyQ5m4HfLJsl3wRECbrVWlw%2BCKpvW%2Ba4i3ov%2BZhiT9LTO4rmCaxAenO160RpAO0mQKm488H4nkDkWnmJ3Z3pS8Y4GaNRMfItxqBREn%2FxKmkozZGYpWtbiIkxRJ3iGkB%2F5WqpQnzk%2B0xtB77PJ8ipQF9Y7RqsLHa5Qh7stTcJLKZtRkjMqP6iWtqdxYmZ4Omm0CHDhKXOBXpsA3sELZWA8dOZ9syTHHpyKJxX2pBFaXDyTJTvFO3RR%2BsYzhjCW7%2Fq1fcG%2FIajH4q3I7g5an1CzWOoBa1EbjupMGyVfbz%2Bb5CPo2dLDvfXwJYQegzJpdTjg9sVdQ2l67BkBrXmhVgVEmw1TqlcO%2Ft4SOTw3XHOLys6izJVjtgigtq4Ptaa9Fvxqlp%2BK0%2FnE8s%2FeTNc9S5sDfos9wlzhwkJP6cDcWeFcgdSwZGesPz0o1%2BUZ7hg5kpNC3dYz%2F4Fu8kApnPsWF4LLTteHTxZlp35vEX%2FUZ%2B6Szh4w6tjnjBMzhVrZJLsKCMuFyqGPzF85tlWil1Jy1M7ELznDWQm3OG5D0ruXOxMWjkoez3awBOEdGrfBsqghrC%2FfPJxLjaVLk%2BgPyzh7LMKj6nMgGOqUBw%2F4jwMlHylYzJCBd8pIcEOO3n0OyzsR9sqlK%2BeYSaIyZ9ikp2PXuiWZHO7fXN%2FybE%2FvzpREUOgQ787kgVaoIUhU048rRJjDg963SDh6gxqylzUdFQ%2B6ZSHsduD6McLOj6qIiYetTwP%2FRG3fn%2ByPWy%2FMTKC6C4Cyu1RIn4LVuiUklIjc1zMYfq8MA0yimqYBoEdyjWEm%2Fy4YvzwvZgEYnDXRNcihm&X-Amz-Signature=5a25f17f9c7091916660f4134348be7dee309c5617ccde170f58a753659f9ece&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

