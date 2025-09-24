---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWERFRW2%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICX6t4b0%2FFDbdh%2BmVpCsMdwJPs2EIi0YbN32j6LJVnYPAiBxWO2MpHX5iIAPR7Jip6aSfo0yrQR%2BiNwtndCZ%2Fc45nCr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMI62%2BJ6cox4jng8vxKtwDslWrWtQQdvHQrjvmSxcDMC62L7JWrGEi0%2Fwo%2BLmepUHhgNiA1o%2BBBQP1Yi3sVlhznKtGZB9oavPZpUzCsjxW8zJGdp0p%2F6uX6zOUSpL2HVnAY1t%2Ffl%2FuVn7IkOVP9JUOStIZIbIijG6gYU0MT3X2kjZryXOUQGKjIFqZIMxyXVB46SbSzgJm%2BgXSjzWScXmMItibZ2KU7hxhFWGKDpljEErsIN%2F2Rs6D1Ptmp%2BTDbFG62hLR%2FM5XL4th1FWQ%2BsZqOTW%2F7KaimBeoXnHHC5NVoe2l%2BG6943cL8e66qhir6%2BjOURwNWjS4VSJyCkmGV3uKIfJNJKRbV%2FyslzJorQ%2BM9K6CEG7Qz%2BWmpTNqZyNJFRLmyfJn2WIOnlYBWX6P2Mr9QkqR4UGegPHWkh67Zo3g5vm1mspDG%2FMGS5GT4TVxDdwtrlQ%2BzZxLjVwBtPvsL9BLcD71GhsaHq8VprjZTLoH457xFwo63eW%2FhLjSYevg7bGV3SRnNgEtAgYXXw4uwIxnBBiv19W9HGIu2%2F2miZWdzr6%2BJE5nNw%2BAsOffFXfUGas%2BWxa824y5KwU%2BTP5b%2FFeyE1K0sl7wqLmrqvLZr%2BK3yQbS5X682VIq4SgQJ620H91FOj6L%2BK6BnriEFV4w2PjPxgY6pgERkknxieXicX5f1k0n%2BAQ3wmmE%2BNUKiFZwRRdRwiWjF6PlfTCQdBW3KWoZrFXDhC0quh4AhfXQUCpvCcFujvhqJx3usyW2%2FWOBwd9o2qLJNVH0hv8lIrxB8KdkR%2BCwlz%2F%2BDZclRomN%2BiJD%2Fg7fTz5qKn5NF%2FhRHLHCwnZN08WS30NN1UdaSlWnB5%2FIfwruUzIIXPSUhOqaFHk%2B8CdclmX8xDrtcrbp&X-Amz-Signature=8b030f85bf63533c3ebe3f32bb19d145e933cd9b5430cb4247f3c87208f7edff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

