---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HWITDHK%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA8cutE8BAo%2BtX4Jf0KCkVB5BUYe1Nwt%2FaWfldOWVbv3AiEA27Kf9ewKYpHQoVzl6H1QHD9zwB%2BzUcDPU%2BWGxiSS45Yq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDLKsy0sFQP%2B30oXguCrcAw9A4YzyfiW3Rsj7Iszqn4lgXjksan0ORJZthODpjvPWJEUxFl2Q7SVbWmwa%2BbGQdjIOxW9psV0zb%2BXo0bndLy16WyzzIN6KMXW%2FB44M%2FBf20jvX07tTLXUjlUn0IHu5%2BVDQbvRsVAbYyQqu0F1xG0FnIZVqpcq69iiRvciIU5NFoR0KfPoZxcSJfijaIGCF3Vp2kZYYFxzo1BEhXpzVxHnT1CsXEe6SREIPqJsWq7radqLWf6ZFIaskmXJWGJyJqpPUJJXv90rPBcwJ1WmZK%2BPvQRxJRThnKwJzXHNLzho2WlDIRCaG6ntoKiansaLH1GUH0qDWbdeZziq4ZlWR3QR7DuXSzR5BnGHPzBwovafGv0FP5SnjPQzmTgJDRN56bl7tJRbqTZ2ZOPqqNdR3EycG6Fhc3ZNFnAgYi4DXT3WYK9Y67cXG0T3Fs2eoZHwM6qtvwD2RL%2FpIMfSsXWLQvGqaBqI23yM8QLyAJJhe3NPWaJaoEeAHAuXLhDJsh2WNnQKDI%2BipM%2FzigMptOjDTm6Ja8hU4saujhVSRWB9UIrkSiN6v1Qfi305wZzaouiKSxwbl3yodTmn0C0vcy85gNuFJGt5jTHW2daheKgXaYPEYpi75%2BDI7KUQIxbZ%2FMJLEu8cGOqUBWqe4h0vRKPtrL10ZUntb5TPHK71jhrrO3UUJ6GWGkRvIwL5%2FSWKbKs5efCIc3WFA6wfoeubhWva8gxbceasv4%2BKNX%2B7AL9aKLy%2B4tq%2FdYNijypC%2FgRgeFzw79b2LMkx2oc8foVXQmDZJhF9JlxbLh8OX8d4ChiAUGwv304EHkoDoNDP%2BqYUh4cchTrSJsKhX7sXNr7FdJ3quFvE128%2F2FmbeRH9Z&X-Amz-Signature=b24a13ad594d6c6976c09c0a3901be28be7ea6ea5e083905032f56fc0f7a497f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

