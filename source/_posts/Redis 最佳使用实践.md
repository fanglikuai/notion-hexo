---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YK7EC34I%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJIMEYCIQDDkb59dCm5eCqFF1DTtlJuA8DRonJByjgooZy8eV6QtwIhAN9Yyxa5kryIWkEgnp4aLt2F2Pd%2BjPxoHp8A4%2BF825RDKogECPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwGD1pZC5TbooYIh3oq3AOH5HXlXeI9YF7FwiNI1RM1Cg5WrmimFnWCpqcicBMUBibVFuZvsWItOVsZUkD0Pzp3L3Dx%2BmmXqF%2Bu84T7CwXh6kvc%2B5aP5oll%2FOCU1yCr%2FuJeMUrIlipoL42EzA7F%2Btlvvu4hHZgR3%2B%2B5SwbdfS%2FSBRZjX0tYFhH1ZJS5QKnsnqlXYokM2iPZJmjbmheTFNPWC1sSjjrC5ozui%2FmIdHhdYrei%2BNHFriQsjpkqmcMUU13mr2Y8%2BVRZFrYFlZhb%2F1MY4G2NOd%2Bxin5MKOs0qzu5tcFLrx0P%2B4rvTf8yVV7%2Fu8mdYwsHmJ5AoWsUV674jKu7SgNOQ9O6bNcP8fec64U5FZv8JtfgHT3DONmBJF8URdWHYcDZLyxFPyx0JvoEVss%2BXas1T13Xm50TacHYxM8jyPIZ9giRTXhnPsmg0PqkGpms7w9BOhxXX0WTNbfOuPg1e9Uj79qhmaGokJ3Huj1K9yAN0MyrHszydwVhDJrwQQYw8Lu1HALcGViuvf%2FeCIPd86fkbE8m3O0pYBU5YWTTfpwsvcnOfljRTUCYS2SY%2F3kFIYVFnAn%2BtTeHl08EJV1JdDQ1xDVDlk9YC2tolgUc2b%2FE9f9JANPP7YRUg6o5d505V7uFPSrf37aHZTD3jY%2FIBjqkARiHTMiSmDeVyRNOKnoJA5I7dj%2BBBDx7tsm9FuQQVeflfWO%2BapcTtv0TecOhhcroyl6iWY6iiCKejGL%2F2NTW8Qf1MTW98RQ5stbL0Je3B8vnUjijeEYwS4pM15gHAOMsTTE6hXgJUkjwEI89aUSqY3kY%2Bxr1PdvXjODXF7Pwg1Bp5IunECFoa%2BlGTFfICl7X98iY1zP8AqZQ%2BbsxOIjPQjnVTVaX&X-Amz-Signature=8e8ed37857859334256a2d29f4e268b5668de5f2832997c053377487c3a98dac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

