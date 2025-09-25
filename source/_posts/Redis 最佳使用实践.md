---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQOP3FUD%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOugAc%2BPVDtqtU4yF3mrq2z%2F%2BkiB1fY%2F19kMlGTGgM6gIgE0cJx0bq6LXtvLIYxw64xtvAQlSOWDF1fVWiYOclvnUq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDA0BFuPaUWo%2BBTTE8ircAyWrIRB2QvwYqyJ1l3qZi4ENekcD7saIwFURewWKi7Nom%2Bo1tUA9syFUZhXps3qtuslWqKqHr5oqRX4tDnuwXveBQn3s8EiKNJUk%2BNJYomnLu2mA49BVYhzm9Fi2AGHAUOEEL%2FPSwY5ScK3jexVTPV98hlOhCFlRkyz%2Buoqht5MLH6rrUXgLAGxGlP8W%2FPEp2ubl7GHw0lAhi4doGxF1n38woSRLf%2BjXVSrOr8UVXcQWjUb60nDRuJ%2BILTB5aA67VgFa05Ztlrhz9x%2F78b4RM98jCJN43crE43YL5llkOqiB3K7F6jaqdD2QSWY43GH%2BWUbt9MVxkT%2Fun13nPraNjKCjHdtJQOse7mU92b1oNArWvUDF35TGM65EEDlFOSm%2Fk5FEcTeBYw8hTMvyu20ECDiaWACgz1%2FF0JZ8glMbTE6sFnuMhVquhDD%2FOJd74m7XQShfdh3eSQUF%2FP1xfxGlnSPJJCogbFKNWstaXCo1%2BfpQFNbTC54DjFsTWBjWjFZwfN5p1qOCe71EL2Nqb5uPDlkz9ESQulBwHPwSWMUGXX5piPel0fO31NU5oeDZs2DJZAdlHpxZspjYQi18xbCfFgahK8PqdOvsNwU5tKtRIF4edAYNuDgamqikKa6nMObo0cYGOqUB0iYhLKfMEGTd1mCTxPtE75rbfuETB523Q%2F9PoUfbbLY1C0jhJxC0RZQ0QzohqY9oYXaKiMZ3G3udR60JUkdPfxja9XbSlHI5Q8OBGJ2u9e397GzLlx0Vwp88BQcMTTSMcDwlYDT89llHY0tx6CaDDJR6xNHgRV3GPyzSJFyxIvZpNF5wSZ2T1vB39pvE2g3t%2Fg6ZWqnFFlCD1Ti922uEu4cr%2BM32&X-Amz-Signature=6bc957e838fd5365df749644bf1c76b9c357f0e0338022430dfe9504d243e83e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

