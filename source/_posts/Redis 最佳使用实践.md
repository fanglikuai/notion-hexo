---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFI2FRNY%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHeMCgyCzu4KeXcGEtA5kT6mkqN%2BXcDxkWPs9sQAI7YvAiEA9t5z1oFQjNzuecX%2B4JhY4SmbRk26nuBYKfFMc43SdvQq%2FwMIGxAAGgw2Mzc0MjMxODM4MDUiDDrDaPi3DOLfSKGUDSrcA2NRigtB4oa%2FHy14I6Kd01uOtv1nrq6zHtj5ECqUAEVkGwBbYyMN73J2Z0pQV1AVLGqbuNckIzmQuRKeaWUPyNvleJCaurPPOECqKIYchU2Ftr6uH7NsIK1IZfs753aUuqZ6QKR1ieK1TUK%2B1SKOqpGz37Oq%2F0y17ooGJMirwoJGDBC8RLCf68VMfimYys2%2FoXP5OGdXWFjWDwrBKzOPt5auA7ZJtquS%2FqZxCNnw1CIxeB4hN%2Bn4lSkFBLVNjEogC8G9xpNB%2Br8%2BQyQo7M2%2FEFMqbtV7IGH5PjhPdSpTV%2BdVDCKjeEsODOsyxT8krmCPX%2BDnyxU1UjwDK2%2B2O1U7oz2HNoScY8eKrqb4lpdPQmlFzBtDUtccS%2B1Dpyzg9EnoQPInI8yAvQBTYZGyJs06BSAvTzg8loibzM8muKGyR847QfqCLjYZNEyNR9oO04rT8Kk8KNdwy04duL8%2FJmO4oLD4CQ7k0qaOcfNBfFcukCH1pgQq1PPtL8EQiPTbHA8ecGa7THw6gsbvxdjSn4MlqHgvbf%2BgRn4EZ7ggTL9KyIfUWO%2B3wCQ5MKi6LNx2eYKCtD8haSLbQgp3hkA2fB7KkbaL2JvRVlYV7wWJGuu1wEEheHhh5WFZI8x0UQkpMMfQ9cYGOqUB5KqDkH%2BpBTyYG93c0%2ByTRih6BWiPNpt8lRhpH%2F0IlqE5raplPp5a8Eh%2FI6GRCIz6PMnalAkMckQjjVOPQVztYdWUl2QoltjYYJyTPlXhIo0jaT2Qv4XYIJ%2Biv8Ps%2Fp1BUH2rHa9O4KM4yszLyJiosJoB9iWlHMyeMi29XbXyu7XV%2BPC%2FHZ0qX1nVNni%2BUnr8906w0D1yvbV2w4HIPjUY4UOtBFub&X-Amz-Signature=8c7c8ace33768ed55c686f1d02d774e0c1157c9d276df4f3d79fb59b3464d50b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

