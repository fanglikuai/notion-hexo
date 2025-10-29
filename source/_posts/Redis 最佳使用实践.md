---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIAZHTRT%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIFkLKS41DDR1ism0bM6WSWeb19d4yB5MOxnePYNBlIpaAiEApHLnsgFNArbTY0Ha02Tg%2Bzllxs3KsuS5iuMxmom0nUkqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEsv4ALLCZ26SDnWyrcA4eajXcLSq3PkpVYiB2L1i4t%2FeqronX5t5Cc5bWaWK59QbuDCLutxyGqMN%2B%2FdNsFrMmb5gbsEoIlecOj0jEVBeW%2FWHQfcQbwD79NNLW8s1F3N7vIimHJJIfPBPF0jXXJYvD4W4CHE%2BRs4KjcbaioiLcAqC7LP%2BAJIIjpKvnkG4%2Bj79qEt1avxv2EvvyZ6%2F1pOwp3Ix%2BZbBIHQf6zSYHho%2BnscJp6rsTVzYiqbIUb4ZrDz7jLbd%2FmIxB%2BJwN3h1MAuHNDaB5LZD8V%2Fbogt7fJAENlLxLB6LQ2GO45ThtytdyFog1%2Fa%2FJKr%2FFZGQTmvRDYCOMUr%2Fqu5P942WZ61Q266F%2FRSeJcgK25YHuh%2FeO3wIqGhlulDcl6jeZEg1Ic69tOmMb943R5o3FnK1YP1521qPyA9f4iEYme51%2B74wyZolA87%2BiwmDkxUKyAeclrE7AehBX%2FpXCTf3oDe13oUusFZ%2BJ67uR696KfB9ES47H7U1vABCYKBjns5qv07E62BmzAVO9tBaycVfwd1gZlFVWX2mqOH3%2FWfjRj5fR7N1HUhACiyvYDUP8qlWdzUfUssc7JU2PXYBbz6%2FBkO1ChcFv%2BtNxRhe%2Fs0vHx%2BoBYsFQodb5rt94j%2FWFeQBNtDA5lMNWEhsgGOqUBymVdrFl4NNdHbaWhti7kF3279Bfq4yKr1Dk%2BFOTIfPZWj2siRLRfhpUnQ%2B33PbB3pwxdRsIZXXa%2BOdotgxHlbosMrt0Bdoj%2FO7KmTn1P50l8zcY3X1VI%2BJ4anQcIie%2FvoYxFksJEB7gdRvWg8H%2BpHUnIA1ip%2BR8%2B4DhTen4kbaScqgjxu6JYAGpus3BCg8cYDBAnJ6gmG1tbKVqAiInEHP%2Fmzfph&X-Amz-Signature=b36714870d9483311d63260f3d879e72c5dd5f87bb6a512bc1e5c60be802b22c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

