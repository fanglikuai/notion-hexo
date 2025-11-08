---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7COQK6%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIFAfx1qVqOzD4vqznSgQCvcCE5Gxu9XDcBgjx1dnNUrhAiABOy2Iaj2xWhLF85pA5hZYGwKtgfxIIrrQKR7ZtvOO5SqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGQAtBONpdKD6ChJSKtwDNB%2B3ox%2BSaejV%2FSmefzZmTXX86L1bYVNIBHX8EjMpHPnmCPXglOUKZqcd9UHANPOhydpGmNE185WKY53yIcEDOLUdJlpvnic2TYxyJ7v8GmAUWf6iUiBN5N2dW%2FyCn5nI6XKeL13%2BAHi91ivx%2FMFRXUltIN0VcTV0V0%2BMmAMcJdnnDNg7tKc1TDlJab0w0m91Q07fIkgzRRyQAVqpbYe562PDoLOiCCW%2Ba8dRg4Yge4zn0j7w8M9VGNrAwojdRII9WkUu7sm%2BtFChn0Yng1cD0eoF%2BmEU%2BoDX4gfrAvWShaARjhytfAONST4B97B98XkLBMISPclCLil74TYzkv%2F3wsdTO8vSoxots8zvdQ4ZlsLowbH5NZ3uEnYNzTEqOn9767pee1aAdPvUEjmQFM46K9usg5wjZ5%2B%2Bn0Km9u9j%2FCYt16iIqdxl3eo%2Betd4fZ96Q1MF2c2wppsjKRiwegTiE9oNHfvtXDOyQUuwJa%2BhjJuQo9foIB1No9CN3FW0sNYTwre2URcYf7vEcj3B52pXd0LbQLCynREWsb%2Fls9IA1dU0fWjxGXfHk3wCt8cyKpwdy1bNzkP%2BfaOligdT3wQETJ6Ic4jiizAI3SQ5w6DCAW%2Ff2PyjxhZ5G%2Bq4FqUwre67yAY6pgHQaZ7pIadooyVPBskgaY%2FXGlTmvo9TjlFHhEPYwGp%2BRJZlDDG%2BBVqK24WvkVHjsdGrtpJwM2imT9Gn6b8hESgF9IX0pvlxLVr0fMVi75JWfZJ2ML7jlMGkQzRQvRHb4HFXUpyPCSUjCc0TvnSBsOs8PrpwCCgqD40ujCRkmRSUXZPhy4NZrCln32TAhH8%2B3bMqSWrEbBzsvhnzpTf%2FR0f8zEsPrcKZ&X-Amz-Signature=86132722ff24d89db2d5c3fb6c9006a17b75ae9abf65f1be64a35b732d4167e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

