---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DEJBU65%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiXcStMOZh3n%2FxAlC7K1bH3lQLsklVBRH42lIkjRyJlgIhAIO1JJkNGCwlPo4QZFP7vsvge3BpH0MgNST8UDUZfLw8KogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIYSVYeth1ejLwgCUq3AMAEHjlpXz6aYsTOR7m0Zx4IhJNW2VCK9e6fQ6HvzSOnt3cRQdOBF5iLlBlhf5PeBK68Q9Zilic%2F50tsqIyo1EqltJSJmnrRIy90Yj1JEscI1zgCNMX2dQorsOugdFdGzJudjckLTUl6cG8CqQfFBCfg8h6FV9O%2FON33Dc557LZroCahE9mo4T7LZivHBu8O8XAtRkiiXxocURVuxzFi5z2f5%2FDaUE%2F3ryQdDAhJ%2BqUfK4W%2BSzeTMGc60jM94cNJh14IkkqqDZAAYG4604VtJMAzfqCxl9dv2cf9l4N8F1MKCffm%2BiLAA8bU5OJyfJCekNEASr1lbZfXJyvr4hYEgS%2BiJHMj2Z9ZPH0bilxFhaXkwtGsyRSIb39fXNLj7zauMcoJIAqRRKnO4g7bpzL47OnIFCVVZWHMcWefvWh0ph0IyPJG3VEi5h3ASZ0njwrp4%2BjkR%2Bb9TLjYVuIZ0oZPpX8GYMXEYFtUN7MSdNadPQZYCk9%2BHYiNw%2Fy4Erw5ApK9NSpMwxALkr2SM9bzuOjECzoLegfAmBSoTip6ythGGHjB4HQHHYTHbhvWJF%2F0ptdhULNPjQ2%2BKHU3g9nCDORu4UFlKMdmncU59M%2FDuIj72YPCKSMM1p60J1o9kog9TC5kv3HBjqkAehZa8%2FPLFsWkrbQTfyIW30nyko%2Fl2T4Xh%2F9971qJgg9jVRxeroJtPW5JTH2cmHb%2Fih%2FRmmNUEb1jM8MPWB3PhZ3Gs%2BTxmiNfJtwH85hLg6k6lGN4J%2B9WGLszdM%2BaURnpui9FpsqAJymBAnf9%2FdSdixBBvpffhWCW8WWy6jeKR3Nm6KRmDdGkKCUZKkLD%2Fg2yNHykUtybOfaRpkiwoXzhJ25V2m2&X-Amz-Signature=732e7718fa57817019ced9012284040091f848a0bb8fb7e70f1ac5aef2efedd9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

