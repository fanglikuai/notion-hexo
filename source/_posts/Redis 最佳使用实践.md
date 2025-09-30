---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZNO7O3U%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJGMEQCIHdTaOjxas3bqBYGwRSqqAWBT7%2B0fIndVUNzVwyV5cV9AiBMqszgvWsP2k5cG6B1FDBIPqtMZIfqe2DyAPIcJ3ISBCqIBAjl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDFtol7%2FmxfUh52miKtwDcjmmkucJkjf9Q42pDL3rm9OeEkW4hGKJAhtucPHEOsDQfSSmJi22EEhKceJqda960vb4l6ef3o2SYwY%2FtxzHMBQolyNYKHzVXW1tl8k1v%2F33Yg6XuZ5cHWa77M1VamitVYQP3wyuVy2f07x5yl1Qth6r1XfuuebNguSSri1XHg4b9aOadsP%2BvhFl8LzMUgKqUnC5jH9i1CB4ZSsdX8LTgv1B5b%2FbSwdrztjcD5BH7FdHGGwJGofYzwDTTacPRz4%2BWJIYBsGM1p%2BknPd%2Bxj%2BEzROQnSpieiNYYRPmrCWf9gP9PPb2%2BWT8epYHmEUDeTxqWdJytw0YEZ6zajHpa7zyuQ0pYa0lHU6IRQBBrFzmRrWHU9kM2Qn1H0tFd8fZOX1IecdHYb93SN8V7E0KsAd7ft6f8xV76s0F1ecPwV3Y0MheGCU7sRLpSlcvjCUItt1cBL63V9%2F34n0GQN5KwnTxNZeRO4N3Ql37k1%2BSsH7e9CXi1D%2Fizv%2F8CFBltkMNHOEW9Omg428vZXXZ2a2j9X5NH%2FJJVWwKBTVPYxd62fNBAsZpkInrbJBXfP%2B%2FgLB37hCVePEZy5vuwD09CezfJEx5LoINey1xbiwzwCC%2FJfOM9mJbxwEfL1WxAMvHejEw6qftxgY6pgFHdlRbrJXeibe7c5p6JYdQMUQUnYYGc%2FAVyhVDsB8Fb4EDr1QSJbAlEMAgH1NeAowod1cxe7kMPeAhxD2q4O9wAZyr9WDdPZZdhhl0AQfaY8eAl82QR6ljZtkwV2mwAmW%2BkPt9esUZS81T4Cw%2B382KqW14bOA17GnLiY3kIo5LikP6nozpW4LS9gqhBxqo%2FleJPbbdcCj5AFWjdw5IBqewOXwN1ruX&X-Amz-Signature=a37a9172f611c34ab7eed75701ab655ffb938534dcd9a31c1ae78987ec312d7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

