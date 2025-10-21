---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJHTKUBD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T210253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQC4gq%2BjhilUiH1scIY1HT8x%2BZdKZNb7U1pxCYll%2BIP6EwIhAI6uX8L2iSIhxo9wzDKwXOdodb942pBUJ1L%2BfEJpmNEzKv8DCB0QABoMNjM3NDIzMTgzODA1Igwo4ShUMi%2BojPoGoPwq3AP3wxNywfGVl19G4ywZplrr9embDndBMqAJ%2FoMyYKTu21h7DYQXCua14hTjx7fNGv0S91%2FH4Cz%2BoOoh9sj0fRmAx5CWt5sCXSq3RVRhdojHjSOSSaRGf8fc8%2F2fdxLOpSChQz0S95X2RT%2FSQ1FvuwwF3pWHRuYR7ZEIWWF9O8DJiuEukI7SGLMlRh2URzBpTkwBLzt6fCQNcnTebDFMy6ZnDrwbUjAledKZAgtRLYwpNfOwPg2cJ8Ti6CgCuHrlUtGBqpNUOSC6iAZqIMTKeiknf0b9KAZ9u%2BAcmNTY1gYOM8U90uVwGmsEY1HUt%2BXr4oyR8vtBVweqEbnT5AyT%2BrfkdL2qVdIV2hC39%2BkMPzVQRW5nbU5FpsALmZcmB4dlFKgcpFrsH%2BIyMo8SbdHg1T6HZEKzXs%2FljSVHee%2BVdX0T2nG7szNJi5Sg5vZftY6pNYGdgozhE%2F%2Bj25Xul%2B1p7mW0nSlgatDVHzdppgwtjkbueSODP8XDqUDMOiH6maLIB0EflZrsmkvMcGEg7rXhFifvdaItqrYZ5KsVrv3MDTpqcPMe9k48EYhU4dJLOgmW0X03rrYKukrrFTXGgTZ%2BcfQfvq4glehdYXGJLp8clCG4Ub1FaIc6nB1wR28RZDDL09%2FHBjqkARWOlFJw%2FY%2FutAzsiazxiL%2FFSUELw2rgTCdqKUJtkGzzI3QscsTt0PVN4zZcLUrEPA9p%2BzS1hkDyCHEWsKVWPycOVwI%2F%2BOEe99VtEPgS2DJVxXFVa%2BAvRsHAZedLXpJOEfQgyHeVZ5VIINum%2Bw6SNifJ7%2FvWLH5jU3mgwjVzZz0Ln185gV08uzp77usxVO1fiTItzvPyDKyn3jhS6ULYm1CCgVEl&X-Amz-Signature=4326935cd2976dcbcfcef4643eb0908885035b8924608a18233b4797601f3370&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

