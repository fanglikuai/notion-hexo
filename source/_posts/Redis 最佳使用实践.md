---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HENSDD%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIHLooJL39329dx3oYq8GvPUxVhOANeQOtwiMg2%2FKOlUVAiEA8Ozt2XS3ilnGOJA9iaPYoyh7OpQArZMXC%2FgZihQhCSgqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIj9zeSniFdUMPkGsyrcA%2B0EwhIQU3yejOGnZp6ce23UgG397EWwDk8fmFWL7BsjbahZhPwjtrI5VlqqNhLcerInd90S47Z76Ld5s6Rrf0UkUBWu4W4jgxy4ftoLimgEz%2FQRelteifOLFZVxfcu8wEJOmGCaMqqsVZU9Xo2z9wC8DObiO6q%2Fyh64q%2FiC41ZRnAiK0gBjo38JbcL71Hll245QhFklduYEcqVpNy9xECfoN%2BP0lgbVE57nXy8xduGLguYDCA%2Bb0pGs93EERxW3kgmcGJs78SyX7piswNzLb7fkeSWwHy0OHpWPZI62rjDVFmmm7c0%2BoCWvgR0ZOH1wSwUQWLL%2BK8h1c5XX4n5lRyxe9mNSSTQ72SkB%2FNo9b5JzAiyGMKvfedTb%2BVDYUu09mbOLqqQ%2B2uBC354DgI1vT%2F%2BH1mtqh4EchCVV326eLhz%2FX%2BA87ju1xCZgA9U1bQ2CBU2BueYv9dvrm9lJDDV%2FUhc50GyNlpAo7YqW3WyX2BWj%2Bterl278C0%2FTKzeDKQOCYWsRdWK572oIIomIzQLALzJ73jNSnnOWdeJp9nFwXe9NaK3mv7htta3SbjZXCU93dTnERkI%2F010KFA%2BK2tCPEsny2LT7YEzS6T3ENVF%2B1Gk0ow%2BwS%2FMBbKFZvgVyML35sMYGOqUB6v%2Fck2dX4xpa2aGQugnqrjtyry0qkXzds8jM6WzEtuTyZsgAp54o9sfIQnCpzrO28unf%2FrBZL1l1%2Ft1PzJvSnQXtf%2BdYSc8Uqva2WJdyvCcPpfyNE4zyYdST77VEmCLGYCu6uNvvACSR8BfVI8OcUZsfd6bxJc6fku1Oz2oS9aqXXF0hr0qAQ7AxNiQTHD2KNIrx03o%2BlhhNSMHHs52l1SJLQTtS&X-Amz-Signature=7bcbd2de52dc748bf9d28ee08b3205dfcbfd0625bf06270f1349db512ef86889&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

