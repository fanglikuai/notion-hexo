---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DPAKRQA%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T160039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCcB9cd0EKnjaupmEOIxJA4uveMLjs0uZtHs6p7SYWhTwIhAJiUp5UHhxSn4yyGxRMy9%2FUwS4upnrlnNwBwy5Lv9XiMKogECPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwH%2FRzla1Ixfle0T7Mq3AOjhXRNLq9fLq5M9XjsyV6b8JdYDitDSxva1pZov%2FQgCgZD%2Bz41%2BIaTOxgAtEELNbfJyFaspSMRU0%2Bjp7wm37OPNE1AVYtmwZix927TL%2B2KDgWRDoxrj4OsmDmmGh8TxPuiEk8vxI7rs91ZDa6Jeur8HokL4etN%2F5YjQ8RRqO0xcEqqG5z4K2NZGhFSdx8tGetf6J5cLtMNKRyp4ZJER0LhufKU%2Bqmyj5%2BBbfRh2UpAV9R5Yd800Ud1xXndRtUy4X6dmURjMTXPCmBYFC7aTwHLU4e64cjC9QtQ%2BsezJvZHRDL08IX7lxLfb8togkLr4wEJ1VuB4%2F6rqMTQ6LDfWYkbAO7lch%2BEcpzTyWTbfd%2B97kSXZARKH8QccEnvPo65DolqFfnzsWBiegdveJZ5XIAGGC4Lq4vrhaki3Z3NXQ%2FoXP%2BqTxnQXRRfeFlmg1QEKtxSd5XfBCWn9%2BVIkvSwtlVxFA%2BISDc0EbtDap6xeUcx4xxvckpSyy3rX92eZWYsaI%2BZ4DQoUVMyuHUENrEZ72ZlaeGMM7QlZmZio4MT%2BntW0QlOjHQI%2ForfqEcHqMPgQ%2FnXd7otYn7LoVihLROAHe0AdM9YqjHEIRwsORhAKPUoX1DUOWHi8pJkU4O4cjCxt9nHBjqkAV7U%2BXeb7a1nhL9ZJ%2B6HQlEeJe57m64T95A07UM6Udt56GbYe%2BDwZ9SB%2BzZR6SOAinPH%2BAccr5I2K5pfcXYS0G8I7XX39vO6ySpPyj0R8SFgufXFEUGr0qKfrx8cFH4FnnsvRejIyYq3DIOMRehGAFkppgs9uFRLQv0t79KIHOlmqz69OL3JdLZ22na4tnfGph4x%2Bi4bTicPmmI%2Fozk7kCOLy%2BaY&X-Amz-Signature=6f50ea9532aa9faa75e8f19066136b20867e9064b4787b270cf0c44c5843787e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

