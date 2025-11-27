---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FJSMTAR%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB6KopfUNGgDGz6jzcuk0G2OAQo6qgX9CdeMlsJ%2FzyTrAiBDXQaTTSsh6BPQlXd4ZPGy%2FdzRjqevsRg3I7cjRTlJDCqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMB6QS%2B8jvmr6%2BR2KGKtwDv%2FYfGiyMgLSDSBfO9UG68rBMHL12F6Blz9bOfL5hFGiTbLBlxAnmPm5dkGcOaOtmpxbjBJVEiTAEXpjqekOlyJxl8ETIww2VxiuM8%2FA8BqoN4E5JglWxaJYWwQpfLhEuzgcYAx%2FVZ1IBqkwOtYzf%2B8D0xOtZKF9USA3OOkiEZibKTgAQDXCvokDicwyxkQahdtMoiNMMZPpLLRt78gKc%2F2rS%2Bew9bS5vhKlGaJcn2XZ%2FkBvGhh%2FdGwBxwxP%2Brx2a8dbrOEJXHt6pseC8M9HoVLZpLGXlqyrLKjM%2FJD3dxehYQMBf9D48NO5OgLqYBejGfGqSOF4cCrJG4ychM3gSLEgs0Il8ndfvkWdoboTS%2FMXgCqRcJW0hILsBf1m6Bcz7wI9Oytj%2FxDu7MYqDXxG6OKb9w%2BRPbeiyo3aDDlnEsGnyCF3TEoa1sVJyw5HB815fId%2B%2FNxifM2rJ7H9yKl3Nd8DN5lWlFPGJjenVbDYq94WpcZuS3x%2BBuufGCO2NbmyAd%2Fl7aijJjXVyWNyYC4RHhB7o9eRngEIVOgoR7fbASgZTXcL0Q7wEv55oQakT1NsDISvvimJXPpketnSAUBb5XIVXq3NuXZiQHsTiGRrvAgp8Esb4Zz2h%2BGMRxosw8e%2BhyQY6pgGCzUmQ1wm83wMVf6ovMqtxBYrhjcDSqexXcZYK9UlUHCeBCcOpZOg5EiBkE5q0R5SV6%2BQ9mqfP0A7LtZSD%2F41g5U%2Fp4LFR31xh07zVhENjQCovq8J1iiMc%2BuWtDuH32fxqFPLBpqcmDWcDTZ2yHQfohpkawcz0S2Rtf%2BHNjMy7J3NQZtAwbK0u%2Fvr48mmTcvipuu2p%2FotUfudAwYccmFtYraTyYYPT&X-Amz-Signature=724105c9dc2884d6760735e0ef332bc30604df34b5fe990b36ec9ea9bbf616b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

