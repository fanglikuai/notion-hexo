---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XYPJIGRF%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQCpXhTE0GVr8sL%2BIwodqPHlIGzHF10zTEmjRQxWA6R7ZQIgJAaUi90OtmbqTVii%2BRNoIECMHO%2FlYCrCXivqYwZNk5gq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAffxJC5O0TlHfV3QCrcAwP2lJfNyBJ11G44Prpe0x6shbuy%2F2gn9wAcoEQIj8L55nMu26WcSqi9MeaxW328TdwgcQfNbEZLqUC1sDXrdDk0nHBYO7vs8kEQlNw2q8%2BWkYFriQ2OBJ5kInoh5Pmz5EOBGRTLtmXSEWNbdOcbCzz3occXODlgaekjZvSoDxRDFOHWP2A0gCw%2BlakDL62Vq5VgmgD9M5Z8Dqci7UIGBCGYYy1oknWSh8Av%2F8QXH2TLXjAqCXPQhw8548EAYt4Cxs1%2BqlfuKw5kyYHJxV71VM5GXYB1VEs3GatCPjQG4P63O%2BuTAgQhQNdq3gIiy8msPN8psSSOUzIZlnh%2Fh6WiBKTwNIuEMN35GOq%2FT4YXqSm4znSznYFnjrOJwpZR8bXfHavbPrOrOQVhziUqR0gEYiMvFXN5ijQeZnehIe4waHzfR1iXtfYGJt66YnOqarfifKDsMp9YAte9Dy2cnp8bnO85s6EdGmygtEvHep%2BkM7nnIAfFoVMogOD844xwZOJmAIbeS5nTqMgNNLB21t3RLhghpxoGEo3OQiwYXvxECYpOFe142WpNLq2Ql3rHT6McaOyMR6FFxb1B8m4WeGJ0Ppr3M6zFS7ZMmDxSthafzEy7ySOQbSoim6tWMZRLMNL4mMgGOqUBZZCn9YUhx%2B6k56ox750bRMIdQwP%2BpkENjEhUtW%2FCVB6ZVghXPzR%2FZLDNhPxeuaTDT%2FH3sORImhozHWQSRFcY7%2FCmMqTZ2KjLdsdSVmAcfs5eqTrDMLxIScXFR5cAZwxmeArwar9or2IPivt8uj8Xbo%2Fjmi0IqqoxzVivfQxXixIuXgY1Ap%2BhVlpJaeaeqehkA7Rfgy%2FSveZMRls%2FuJi48AuIpEBE&X-Amz-Signature=c8efa520a551ba480cb26edc3525815843c9a53f99f20d0c17552109792a68f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

