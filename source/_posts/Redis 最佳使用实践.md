---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKOPYYHO%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T060053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAdJ5TktI3dBxXdWNeqwb6lKAXD3laSskYAf4ZyzGwzcAiB1Hxoh7b1dCGro3zduyl%2FXY4fqt%2BABW5dhWqk3xtH2YCqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxaRsKPOgNvJkIpnuKtwDYnH%2FvA10YvIKrOsFpsH8P94Ev%2FuoLGURGwiOKF6%2FU7fqt31%2BnGiFdSGEviiUnpnzbTtbIa%2FYb6XXSxVbzowgtqrDDnyrFdVgd2zInz%2BcvOuzGvBnvbl3NgBzvI15cx5scpWIQb4NCv8Yyk39wBxkYFT8TrWrz0sQ%2FrW9p02PXwEfqLmIAo2jRtRnAzM%2BM0HlL%2FaSO34yEVWoG1JwwytKeT5YFD%2FCc%2FdsUOoK4SALCqPd0heNfBqvoGTKlN3ab05Xg2ahuVnTMLI2N0GSAzwbMsYhnJx9i1kPjTM3DlA%2FSHkEdXs83NqpDe%2F1UYa5KOEdVv3gMWHR2bCzPtWK1XcXCV0h%2BgSuZiM3vncSVvSR3OHjU3NhxvYUeuiNC%2Fqsif9zcS4aZlHj9uU5myVj9GOAvvKOTmur%2BOSIc7P%2BxNU%2Fo9Z0ioeQRR5vocs%2FefPZWOJ2wozv6go%2FCsMkMcaLkLp1Z4dU8FYWExZgjBONwVKpob56hP4ExZhfrO4YzXpifxUBmy8LFtvgHHSaOAKBO5F6GHIDLxaucglwpbSnZoLNCSiDUcQ4LowAZzZviOGZbTMXL5gZyjQlrb6R7s4hqX1%2FZ0%2BVf59hJ09qmKRuBUjGUnhqGFrBnv%2BMeT5YmGIwi7ukyQY6pgH6yBZvdR59ugreacnIIuFUaK%2BCIsiK%2BtV6HLB1jfxiYJ%2BJ80xNaWt7NOd4CV%2F6%2BGlCNikl6JGg%2BDhKV6Kp%2F8G%2FfS9gDGY6ZjWBlKsetSAQHutGW77XLlSUjHA5RlQ7u0kZO30xWpAMc0uEr7ODTSI0%2BddbnXSQbiqvMwatWKvmLQU5YkHKpxyM1JmfkrfX1NvABZfl9rovQ7zG6Oq%2Bnq3sr28eUrQf&X-Amz-Signature=6118da4826824f0bd2c78922774084f031c673787e2082904dcce93f812f415d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

