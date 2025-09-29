---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYFTRCTK%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIAQyVZPJdXive9E3oQRpJtsO65k3NLdFX7GR8avdDio%2BAiB6kyW8imm979xNM%2B%2FcqXg2tU%2FjxIeCI8DC%2Bbn9pQMHUyqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkuUeTPR11t5PzSTeKtwDXi8snyu76e1N2o0OF5V%2F%2Fjdt6Y%2FAmIfBdoBv5kBXmbftqdyo9xSyD%2BG2G3n5QoIKeWp%2FLY5HiD2e2t7E8hdCtNPnMEYEohABjIl4HVVoQyI8tJSmr%2BSjoGQEgpdjs0Elr52Ba1hN4tM2gfPE%2F8r3WTPngldFEuC2KznA3q3kJIxJk1WZJHJS2wsIf1y9SEbyxnZrUrCzU87sNAcgS%2FzyZw4jya1pBCp4Ok%2BU84OzjhgnapJLaJKDSfeRGudjK0nAU%2B82S1S%2B0jhOyJ97Z0ZBRQAJcLwky7jf2iu2B6xmxZb%2BZxI7bzvXRsmRzKQcRXFS95eqcmtRXtHO02ft8CJGU8vXNW8ENoU8qreKlM0cDyeSfFbvsJbY7y4DuU1gMj2E3BPP35ywj9vry%2F7mHMUqsX21gOK7Q4AC1mmoQ1uN%2B40m4QfIZ4KqEeekGFdC%2FmD9jwfb5wKrDNbjO2Of0NdfZ2sSkja8lpe5VV7ymrHsBxvF2p625G9QagueteWvIgWPDJEptIN057SmeZU%2FOJp%2BDug4snawt1KkO75xO%2Fy%2BlFS54QbnR2u1c1TwD1ZiqOb8VbXWTyY6Pn%2BUKOyopSiYspfU78ylr2pc%2F6zf0keD%2BxCEAJ%2BclPmBaQk9bFMwjNXqxgY6pgHFaxatAtIzkdzls7nkq%2Bl0QSKkNqfdFefVSeF0RsarG7PIgCs20q6F2Vo%2B20EW%2FPdwNtY33LHvGcusFDhKdFduhiz8geyGC6N83%2F7DA9f%2FNdXmAYKDDaXJEexBD2uvrne8X2c0zmEqY54CGa6WIYH%2FgIT9D4hSXtxwlknr73QHq0oChwK4AXQvnMCxTtKMkME8gazwqAvDDliqDPUz4j9zgA%2BUab9n&X-Amz-Signature=9023c498c10c18d3064afc0e2385fc15f9c67dbd09439ad13b7e735e71744502&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

