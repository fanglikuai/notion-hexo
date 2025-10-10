---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSDNNWZD%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIG4q1BT3AGVqLsGPQcnTDXXpJ5DiH4uEXi82sdbTnZMKAiAhKVB5H9DES83FW7u4HSdh%2B3uKt%2F%2BT%2BmRZmQib3XGJoiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7Nlyg1akrzgS3358KtwDXCRn6DtvQkOcM%2BnshhBK429eCsuuSPX0GmUakEp0jyO00LXLJB6f%2BW5E%2BKWhelLYfa7var7xDwXaaMBqzfKu%2BfEb%2F4oVmQ9%2F%2FVekfhjf9oQ8n12Vyy2X15GLiIHtqSZ13Mecz5Odl%2FIJ10HxChs9cZ9MwDhgcU1CQFBP%2BbJbPuuorddgoh%2F%2FHzHvbBY5j8q8TEnubLi67kDpFsT4DkRaJGuiVffl2AIl7Hv%2FxmWYCw7XdwEWB7bYZ7WN%2BGH0PlCPy2pTDSSJHBKLgb1rYX%2F7fsuI5n9A3ybVcRDvR3bO88YVBB6vBu83oPe%2FvEBL97H%2B%2ByjvncYrKNjGz0731rSBG1BlwKY2MsUdNkN3mcrnKkdEZNQhQu2V04QOJRr9tcpHZ5wQjtIG6mVN60P7Zn3wyOum0uQbFkGvCiExK3BdP2B9MvX2ugk4FF6criysHCS0ZN%2Bnc0%2BwH8itrP191XannK%2BV5e2l2oNrApK0aoBoefoPO8uu8l1bSNGFBmW%2Fx7PU0cGGRX%2F1aD2etwPmEO8NXADAARkwfVK0wEYtEuVD7KOZGpdYdvZLR2kGtIvhirrypmYdiQHr2glKu72ddNuVO3fVHTojMrk7yWR6MYXZDClYk7zN0Bd0YSdrrDUwqdqixwY6pgEqTuApL9B6KV51RJIWzwsHMOjkErtgAG%2F9gIVMO9byujdo7piEGwyxgyJxIQ%2F4aAPYD8lSb06PG96SfWATyyO9ky8YWTfbngHe%2FZZti0pElQT3UNWoBp4mnSqGqbniaXz8%2FrsAfSxTcRUiaxE5AeVdmfZdV6GeKavlC%2F4FK1luWcXhJ5FS6rz9uvFGf6X1uv8ABV%2BjohLDl6Zz36gHgpwMgWRy1eL3&X-Amz-Signature=e972f9a8be86b3d8a3d08f2a2660bad8cf95b2aa175606c7e52ab8ba5d0fd6fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

