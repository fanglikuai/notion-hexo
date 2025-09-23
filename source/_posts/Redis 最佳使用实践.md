---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAGIKH5A%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWKyhPT7uZwhA1lmcNvROsRV67lIrn3MRS%2FxeexA%2BJXQIhAK4KAWeQ%2FTirh8ujDrk9%2B9ov5p1DXTR2tW4l0kW6igqtKv8DCEQQABoMNjM3NDIzMTgzODA1IgzifE3mOHeMl61FQgoq3APFZZEd4Dvu6YPE0wx760StUedQSltT63Lf1cOBZ0InrYwNcHoIpwWvtpL%2BWhqY0sS%2BQ5zx2VyIbcOXfLKq0UOu1x2UV83iDFR4t3qN%2FWxnjWd0vw8bM8z2E8UrK6i6ffZZgHOT4QmxEPzjX6iC7ayEqTjPjZ5xRcQp4nquQnmKZXBE9JQSAJzgG22%2BS%2BursftR97X0fApb20iDoDImB94KSiazzZvgikKbF1e9c1TritldCHJf3seK0Ebtf%2Bs25QAqh%2B9npEctrCSbJ%2Bpzs6cOIeydVCK3PLQQ2xbKDJma0Z0KozexvVjhMMlDETCGu8lqfVqh6Si3G3V4o%2F2yrlzY9n4ali47O%2BNygvrOqmNBPEx1Yna76dFv1r74myG7wfjTLZbM4yKZc3CE9D3B4Igyedh4d9A8%2FRGdDASno50apIZtDexFUblyLcUC6%2FswmQ0RhFCTi9KTSJ33MsHKOZykjX2ee0TMLINo3YOcm9Z4g5xmHPoAQdrIcFAQYDDzqBb980ryUPIESe4%2F6UJKQ2Itr6rqv7MccAqkg7NjpZn5xGvHWRZHG5Grpfgz81p8sxq3uiJut%2B%2FEpRVw2pirZjHvItRXV8awlNq56zJqG8bLmStAtUzFwRxHlhJ1TjDG9snGBjqkAWeAQcCzOmbtbUNw6uIQSJLLhZeFNLoctFVfYaMrvcaDXO28EZQY92OpuvYiU7v%2FJRkZi2zx4nJZFY%2B1fAQZb%2BEGEHdpPtQmwI7Pf%2BbO1L3VQVM%2FGe6IE2%2BbJPOHXmUBdKJbZ%2F%2FbxKg55Nly3Cd6LaFb5PHs8DknY8RG6J6I%2BiMAehdMZEWKqNQNPeyWYlgwJSu68Utuoz0Z19IpUIzEqRrtl69Z&X-Amz-Signature=7abbe970152ffeeba151a7ef46b6418bd45c56d781c2b2b479229f2889314010&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

