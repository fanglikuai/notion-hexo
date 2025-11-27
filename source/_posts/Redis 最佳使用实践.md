---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCLC3IIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCYbF3O8zicU8kicQ612PjeY1JGwxURSXT%2B%2B%2Bbphd8kagIgDhfhU%2FmFjgRyr44%2FoZ%2BzSqNaF5dMmI5ykk1SdIlUptUqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNDmuLROCAS6YFJsBSrcA%2FvSdw1GmcdyzK2kX0DJ9bi%2FMl6%2B%2B7yTZVudDFrFPfFy8bMXnDncFBpqFuXZKKtri5bJRqZ0Yqq8WrMb2c9hAldRbuiRkc62ykfpE4CBRJQfZt%2FEo0bTW0IthH27RErxLjJqVxNHvjWxKwRnJ0Rd3EvtXg5vvd51F3zLsP5%2F%2Fm7LmHxEqT3IM18DBPdQEekOPx6JJwOICRG9HK1RwX5qvS8DJoUM7dwkpeI0dTqAGIJRX3N7VVM18my4tvIEQxi2YwGJ5lWM%2BCTSBDEE1qSr20d8ZO52BwWCVDKtuJm7p1W4RfEofkXpQ%2F0Hn8wsJJkKOKno%2BqqtOjkjkk%2Bqyiou1rTXfADwgxD2xnCKDtkvmPFMqPLVw2Wql57uF2OT5W%2FNQ6b9l0XQGboAQKWC%2B0SuGYNUKXJQBCbBYQfChZXwP7oEfRYrRFbh6vr%2FglHYb74%2FvItC5CAahCKN9PguyY9KWW01YrR%2Bw7Irf2khHIxICwvuUhbnHGU5me2%2BaDp0klTrOig5VD4hdVgv%2FICHzmu1HrzYSb05h1M9ACbdpd3fiy0uwuBq7Wver%2FS4PqNGyDnJNHAaHvhMQnko%2Fa9OhnC8m06xxLcpD2a%2BpkgnR4NDYey0faVlBUJrcnMan2sKMM2koMkGOqUBxGy1UwcrVNDAHmZ2I6mxzO9CrfoIJxnTCRYBgD751DRKaubHRTymFgk2vSr%2BzHammKPSj2e9hXJsBOpgkh9tYlbbpZwg4PRGbFztET6B86z9FJ2Plyu0I5Y4hfgK2H%2BXPSUYZE4gl3yYJHN%2F5S93tX7r%2FTR0Doz5zjxgbmVNsB1kUU6BIiid77aV4p2LOjLY8%2FZTA2BvAQyRQpi78GwsC39fH7lS&X-Amz-Signature=63b773a08bfd36a301e0165edb6fe10682a4e56a89d03a1dd27c7c7f93ca35df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

