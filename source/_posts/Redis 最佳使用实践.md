---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX65JKQO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDdxRbnQtkoaZcvfcHh9eAkvPoIpMT520z3eiURTa16wwIhANeacX2S%2BHfX5rA9XHy9X9jlojUj%2FurRIjV4MVFzB6g3Kv8DCBYQABoMNjM3NDIzMTgzODA1Igx%2B%2BJI9ZhrvRspRKCQq3AMUenTY5SjHgXwQywvxZqUZwNKo3iBTZZSAOPHBdlrQ4tzJoo8QgpFTSg5%2BdCXV9sp1VlAT0o1UhMlhwhzENWadzilpqKSunb6MYaPaZNWXVxzd%2BsoMKMu%2FtLc%2BPvfRhlNFMJHJgHHF5S%2BZ3OKb1SPiaLAyMSwhQepDdaVh94HrNiUy41pqp4hk%2BjBtfOSI%2BF0024cTMNiezWpJppItVWMpolJoabB2Ckn%2FYyS%2F6to2vDRuJR%2Fr9hEDRa%2FpyY3pfkwkycZWdDLGf5hH8Rs5GSyqXEJAqFZPE4SNzCReZ29LLsezMjD3myFM%2FdGXEvGG9D5JhmWbS5yOLOG47BzGtXuDd38J9u6N4WmtjgWbhOAg4EWF0LDKRvjOesaFYOwhZJXPiIyLZvi2VRB6jWzLCDMpnQlutKT1WSzzVuHs2MycufLyJSFpeH6msWpskm3Xw7g8zF0XbSDipCZgsLXD8vyjX0eX846HhRKxPx1Wi49dOGtP8p4mVHdCyHlP1TMWtt34IlSLJlnoz1eSIv9w9RoS843FyaLLQHhi0cpbKu5ZQadoHdhbiPIM5tqd0PIJ46nNX1YPDiSsvs%2BS0IU7mfBanwCikpeL0tZP0qZG5LVvduCtYGIJGsTXqBqkMTD%2BpKnHBjqkASch8kNwPaRPiTZ1tVxQD8ED120brzs5GkKAGkuapYXJZ700GJaR6YxsrMycqh950kygg428cFn8WP8DtRM6cQKrrl%2F6KeUY4HpFAeigbIrilwua3VsE5E6CK24rzZDVvPnXkZcAW6kmBv9iolZp55KtM3MCA%2FQlVsn1IjUtUKMTON02uAhSISxwulh%2BIxlmd8jcoDuiO3AHAYhQgcvRFjaHpg5T&X-Amz-Signature=c76cf4efb3d27c4e136df500169b35632abfb2e22276768ef167f6060f2cb51b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

