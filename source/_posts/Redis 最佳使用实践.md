---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IQ5BJZ3%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXbtsJDuLQwiy2%2FRaKDmGap4L0yMDm2%2FJAz1BLR4ISygIgDd%2FQi%2FQ%2BVhFmifNR1h%2FyagK2W7z6%2Fuh5VoQjgOIGobwqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBy5myaPYGqLs%2BIF7ircA4roR54tHvo2DB64uaubpR7%2B9zIt3v8kf91Ls%2BLA7e68%2B06DCKzynhcF7D5zhPLvunplNekGVt%2FhnuL2ml80C6nqUZy2subRr5mSdfzPtfp%2BZMZkbj%2BdkWzqF8gY1zIz7kfBvtzUDwSXQcYvsNaqifEVJa1zQghEvtSAsPkSVzsNB%2BPZ71c8G9C%2FWXQRSzSG4Rv9vXdln7yfDBooPMu7fVd8ng10VVj1OdZj1QoA4qD38wWmk1ez%2BZTUgRbbssUMwF3yNfxLAajydCWjbfOSi12Z0DcllMeJAI6jQu%2BHmp5sPlxH8JePtvCiELK1U7CZFOBbPC12Kgk%2BhArILF0P%2FX16ntHNwVUQ66IueB8nWC%2Bo%2F0w5BwTKgvOWTgO1u89UsYyGmm1wnnNOFCbxRdB%2FQIRJaFSmRHDtYHKQSWnPgCcK7TayLs82rKgtiYI8vIh8q3LzrKnG48g8ravVB03yFs8VoVdF9KEdkOrXAJ9WM%2BLzMrdGszbA%2ByXH4Z3Ms8I53b%2Bq8BiRxE0q6DDTwOhXHQFFcSpud3ESc6AtLzm7qLkBCMK%2FKym%2BEdDF0pj%2FJSzvdATxHDdSBG%2FTNeheyqiwlT%2BR57wjS8DdR5yd1eH0AAATvL01uINZd9NflwjFMPmvjscGOqUBmCz9yI4wXmWx%2FfaKRdfxR%2B4d2V16yMtywA5v8TVdyMeY%2BbHtcw8EyqG7gA9Z5eWVO5ktePjld7TJEVGGMv4dOPHaSECw3fgCUZT6ef85nF4Htbk%2FohrIXwkN8%2FLTqoCufmXSjKcLtXUyONPO95ID8a4s1zpndwdASV3FYyZsw8q3L%2BsQwauKXWvhSJtYv1SDRWgv0qVWTrXe8pS6B6L5NiedQDoc&X-Amz-Signature=91bc48ef66459ef51b203dda02a4eed7b11ceeab4ca12c1470d918c1a6969843&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

