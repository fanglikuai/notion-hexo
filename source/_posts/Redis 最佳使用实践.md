---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZO2CUOA4%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHJu5cj5OBVlFabyfvKtaoOK9w7qlxgXQc7ZaVMq2a4gIgVN%2BKJU%2FTj%2FqDfY6LQeiZO9aB%2FIAQUabembcukuYfjHgq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDKC99MSsjZQGk4ODHCrcA5DYTTsrfGjjRzN4UzogvYEa43Nko0C4H%2FcqaXjGdu5J%2FNo9Ex1aUoyzK%2FWM2kLNYVYV0%2FuFwRu9Z2m5dXr397X%2BMpxf8w7%2Br8nR%2BVbjOgWaBcp6%2FsufSAD%2FlcMceTHmhPfJ4sRGfwUVkqEbYPlJv%2FxeSfrxUKDwbmr3DqIiUGPT%2B0WiMJnHZzB4OyVB7%2FVoHEVdMysiXoUVBbWE0k3E%2Bj%2FZUwb0oGi2ztBcKilYd4Y7vZ4Hi%2BvJ7VkWErnGlbHRtIUyS1Bty4IwMFnPbDJjyxU%2FiVd4WSdASK8jTIVaACWJYBdPF%2FjMcDookOhQ%2FFzy0Ks641bL4Ih4O3te17zZvlzO7qFrlBBZHfb8wun3MJjTXMOV0cC0M9%2FBN%2B%2BSHWza3LzbTqNjuq2BQEU%2FoSFDHXfB%2FGkI8WV6Fz0PVOq%2B7YsnemcRm2I51%2Bm0%2B2o7wqSB4lXcmJvrS8GT7iGZ5O633ERm8hvuaJGbiMTq0qEIQL5t2mMVYfuW0ypYl4HabH7i06uAmxsEk1aQ%2BqeiPbytYY2P4YQcvqhM6QB%2BT8cqc7%2FO087TSVVpsPuYQt5H850onfrGBmmLMcPZ5YHmFJRXKJJST3iXVvuPhHMK8H2Mfxpj0MYSlmgfqDQNoK%2FLMIeGkckGOqUBNIyoapo8XJaCmpZy6FPulxg8y%2BZb8goS4oUwqbs%2BwEPl5Xltv1nHTVq7hKRXUqhESgtk3OorL7HCfiqXLt%2BJLjADyMExK5QBerIBJ5%2FDmuzg3uQVUG%2BBCEZ0vP8pFvNZEIKHLckCd%2F4mEi6mgw8YUhgdakIvSdjnvw2laQsWh5rqEJrU9Ep6auLANjaCxdHeGatk%2BacpX0z6WD%2FnUCuIJaACseMQ&X-Amz-Signature=a69056e30581f9f6299a6eec46b33b017be21dece868009f664c78d22b55c9b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

