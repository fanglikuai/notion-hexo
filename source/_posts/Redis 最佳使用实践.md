---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4XIZ35G%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHajrup75yOxHNoFFBK7Tzutvys%2B9QVGkGQIho9ABUHEAiAz4hMv4g7nnbq5oRkDJFLY6KMWo1ZGR8946Ns5KCxW5yqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTktgDAEedjAVmirJKtwDpePfm4A313CFh6S582rOoZAWre2z7gYZWV%2BQWnZ1Syd8oN%2BC%2FX822TwDOTXMXW6RKJJj45YXbHiuh0O%2F%2FYUT1tp3vbkD669hnkMRR6Xx9WsDgppTXplTAPUpE7PQUiSGGbvVg9nnCmDaHQ5%2BWdebBljtydyaAz%2Bx3xltj1diWVIRcpVgjH05Wo2yvxjlczUvBI2C4D%2FafFYungKyU9qkBmyYiWyGZTA8mE6VmqjiXnJiWS2Y5aRAXM12r%2FKkaoioQGcezBKHTlrIXDznVHSV4%2FqZ3OCbUx%2BKDZbZJiUYKZiAknlKPyS9QZ8Hq1xjYfW1rO36%2BVf3m8948FfnPKCIs6%2Bg4vpdNpkZsOgXfZUYrFFti4iMjv92qgRahcv%2F3kKENyAOsSPgQA57dUeVvPi%2F%2FbsKPnW6rJi8L5STZ5qvnjE7hgUyk%2FxBeAEfEz3TrTQE8nRuD8DqP%2B1WDYfetT7W5MFipstAMgWPJhqsItWruNGXW43JWNZnXd2zQN%2Fc6PybDfsTVbbpvgNHoD3LrFxY%2BSRxK4mW62IJcdHB94%2FiaSZ3bs6UPuVhQZ1qdhgm3Rdgp6N7Ubl%2BCbDFTHP%2BDz40qIWhIo2ewSJQqHerh6NFAo4%2Fu5cw0T3A1pmMF1MwzZyJyAY6pgF3K5UftpWKGuJdYu5CHFBC6bOTK0zWegBmrj3Jlfso%2FrHBO6XWzNFV%2FlQf2wmMc8zt4Ffewe9L7lUQkPDsrkxMZs0Q%2Bwo3WF5C6UdXanRxTkAkGoqE96xRPgBrDNR7FMQDjmpdOszVjc86lqzzGbhgI4RaJik7cNFTRQA4wBmqSMFw0tmKeer6ERE25CZjuo1VBkfwn5pJm8NXpoort1Q46cFynzc%2B&X-Amz-Signature=fdc52ed8f9a4d6d8788ad55c66595d89bcce688581e91d2b61e9cada81bd10ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

