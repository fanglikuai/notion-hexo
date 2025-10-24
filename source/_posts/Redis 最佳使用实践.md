---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEZEXL2H%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVx%2Bjf1%2BF4U%2FUs9BnH9MeVzoD67mS8iy30gGHv2i%2Bv4AiEA%2FYa8lxPSC2QDRg%2FX0a7eBflQBksh4LencqomoghfqSIq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRNxxdefUAtN0AjFSrcAy1m9FwZXzqxj66vJ%2FPEzJesvNv3GdR6l1XjrOPZK4%2BRH%2FYJeQeAaMchBeUAt64AEzQoGvHfjNXV7y6JaswjhRQRWONd%2FAvOdqh4rbN4VbDE5N8YhO42O6BdtlcvY0yBk3bmitKVtTpO20Jo%2FItjWDpFjnlYsnR9PTVAxEH3v5cty0K6AX6kE2Ecu5st%2FIGDsqLmpP6lhjpG8qiQmfRpJOOlSgUBy6deFSx%2F7f5Trh8G5WPg3t%2FNfUMYsTsVTkq2%2FGPfpseHBbhBqNuY8ROg0YWo0nG93S2TcX1AtrNHaEprzYio%2B2JGSsHCYxrXhu5n9FnCy%2B6fL7qSdPGuRKwawi99zCRpquHqfhTsCbgtavEXld8iMFAjgbZ2DSFLIE8OdgF9FvuwPbCUjtUKukBuYpHJd%2FfwIWfUvvUMlZtgd4iXJ0y0DGcOjVw6w3981bplo6yVZLZRFtZnwIUAqxgJMeTYhoU22QAf2iqCsSDuNnz8q%2B3eviBer9AS%2BfvhsAlRRJvb8WYak3xSKaTJg9%2BBQhplSbf6qBYJUmEskcrBdQMwq7KjDBlYGejzEYWMQMNTiTqwEfhJfm7b0pjYPmlFLJ5mcA2tG%2F7DUbaa3vEGkWtcxDNXNIdmf0nvxvYnMMqu7scGOqUBoBq%2FDX8MdL70s8g3RkVxcOsw5Z3PR4xw1hnhOTzzJrEVsHWX8uQgJWHqjlomfu7tWdrfFNG11f1be65h8QBW9DZmSnmW6AbkdU9l5n34r8LLQxL9qvrdy8VeNNLR3kOSUc%2BinlA7o09CpcnOXnxbe4vcc6UaMzE1ErxhRBBwm4vWZ24k0dsWMtbLAWqYJH%2BDP3ReaD6Aa0fVtrUGki3XzxVuHzaH&X-Amz-Signature=45fb3bc9a5a93f25f0fc4b7f8facafd6045ce537899d3309ed8580e91ef803ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

