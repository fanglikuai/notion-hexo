---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSOLUKZX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9xpGBJwolkcupLCxomIPfb%2FR1tyB6W0rKxjN%2FfxdbBwIgaVHNTI5fi6WodAbmJydfCUh%2F0h1DGd%2Bp%2BO7yS5TGO%2FYq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDAUexFlmW5CfJ7x48SrcA09P7sSXOXz8tZ%2FfjWYG63%2Bdnm8lpWTIjk%2Bo0P3DegjDpSmJ%2FohIinnWHtnsJaoBRV3WMKoZodGYwcKk5g6M5xk%2B4XscH7VNvM9VYgSTlvfkzDEINdrpYzpFv1QhvZ0GdH4sRSqtDlDNj7tmeGfGJxIhVyuBeTVoSTas3yCSM2J0FJJCNOhliriCtd7PZgb1%2FSHdI0nfF%2BGbFtezY91dk8SWy2ng76qeqDeusLmwM3QaIrq%2Bkaql88vZc2JNMitZMs5%2BS5YqQ75EehH%2BztjzXjCp5rXpjTO0PZV5P7EqBkfNl2LfVmpWZ7YV3DdVPi7EJYG5K9ST1NkqdYPjjgjyfHtQxPLuEUwlQ2XcZnrzSXVpJFO8UhwFIkZBXslVm7vy7rVkdgZG5%2FzhbMGRqzlSehc3QQxvv7ADa2AxolRgcHghqWxvo9fw15VMLmkOJNN%2ByoAXaCb1l393LOU1Iac4yH5V9c%2B1%2B4gnCyiKxYmONGsrDwB7ze5SJTaAFgjZgPqQ0Tj8YsIvyUs43%2FFP75lfzw6g1K5hjFgxXW%2FMnR%2FyMTyLz9reZCy0c9CUeP8Iq1BD6AeihKxjCZ6v4VVq8%2BAMn62Lv%2BT%2B9yF6vSAVTkGLSlQ5lEMR%2B%2FGVRiyPmR8GMO3Ps8cGOqUBRdV%2F%2F2mwX4OFJrAnORxLCkAz8wX0urpBlXEmQhkOOA%2FdBqZVtPflH4khxV%2FYe89rJeIQiQesw6%2FtFFdPi2NMZWUUUQiksdamvaBUNtxMIRgZcWgSj6FRES7eoyGb1AE74sSz8S2Vls4KNhydvWpa6Tcr7qDpsOn0g9Ix%2BPQeZunuzdTBMnjg%2F1Yb5CJBBjDzxveOiQvyeLuOMu4JnojmqtsnbXlD&X-Amz-Signature=f7024a1095c829b73b9fca9db8eb2e0150e6eec9c7c99c618c043b0635824efe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

