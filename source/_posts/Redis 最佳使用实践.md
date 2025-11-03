---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W4LO2AQU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC11qxrXiH656JwrCnMfxsP2%2F6zldpsOT7gDerruejScwIgNNWrWYFhG8pLEpFNiDZfTZ%2B%2BAqKqm8GsFMngCXDZWgAq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDMMAxqhLX4qqvzxo9CrcA5vzmRvPYkh9rsS5MGNKOrKLbW6c%2BKD33tjDETzNymbR6xKMKj1w%2FVnmnxokL1jxaKsb4zEEsP98XvufVTZjbPDfR%2B%2BxQ%2FlPqbh5xfbXxhDcxiE9sWSnVOdLINNolWRqrjeGC191uiczyHJKvCwLa4TSPg9yFAa3sOTd7VXP1s%2FmAURLd9cwBZFwAtLD5nAE0H7ALpKTMMAkS1f9iE7agNL42QrxDrvlGTBr9UrfwpTKbeSk15MUc82aHsZ6x8F%2Fq1Zr2%2FBdMIpdRwhBguqDDcAu91fTWHOT9HykGnoQ43TaKkgt6xDBVJjVKS7%2BtetIMdyPnhCb%2F0gohRnJyryG95JPXZam%2BeeosWLqSG8MfcaFd27jvFpiDvWI0ACjJwkOnYuiJDKwaWCi5BotfxDLX0R2b5IbUDunMGuQuB4Rsx62Y%2BCi0qhOyU9yU2Li2RCLKEKqOPvhE%2FfDWR0o4Sf90pCiXcVwDgHCmmUEafN0QOEbyJ%2BTQm560sO%2FuLrOiiCoL6%2Bq9WkLPQMxztbo4J%2FQx0yvRxy2N2%2F%2BLrKblk%2Fxh1U8I5U2DN23vaXVYw1%2FpRcacHGHhG03gY3p27fFlJsSmmoUIBpcx5fxJ%2F8J4VXN3dc63XL3J9GSlbHwnPy6MI%2FapMgGOqUBRdPTUS0MRwVRQgAJgAKjZdAcFFLsHp8ud3Qu2tJSLQcVyo28SukigjeK7bL%2BGalde2ASbH2XVxgZ4K%2FLCt7jZprYQQa7DNeF21cb9GF5OM0nGU71pkM8Ili9o%2F%2Fqz7RPmWC5Xva07XAZu5%2Bze1o0xWEoOUY2NeH8JTYaQhWWSgPVl07XMKBPj%2FYZcVZHv9xI70pOFhY4zogdHe%2BYDdg3lLO5nCtC&X-Amz-Signature=b1b1e9a061dd89e3b1694c25385863466985087cadaae88c79403d91fffe6859&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

