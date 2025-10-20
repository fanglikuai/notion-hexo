---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUGYHPMT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIBi3XFLOiZhkv8ziLbyC2hC0mTFWQQuKlkxpaCb9SI2UAiEA8zIvKRCFYV7hBQulrq9YIsZrHFs46dwF4WJHWTEFwh8qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMK%2Fa9oHQ%2FChpeaqlircA8XOWG7My4lgON4%2FtYGh0sQnQAsAZvX1ym3LlDkEAEWORadfigFDHm4bJrY7FEmXnjMGGdphLX9rh3wK1zKAcBYqNegCPVhESZ9C%2FaoVqCzGtrNb4ZHPX%2F4jx8ccg6L7ptF7mnVCBtS%2BgFwRYtbw%2BNBGenyAmbAYEX3qNdWrJymAQFBErV%2Fx3DS20yFGIkH3HJfgSMtCDQJuT8gV11jHXEQp9zmCRHmWH9Bkn8H72ItPCkVQOCvon1iBj%2Fl0w29jVq4hj7oUoARE5Gpuipt1%2FGUpPOXMDmeEUU5R43cZEz29XJTvIkL6DlysHRtGKpFO0oLnCTulGv6dWpq2HEn%2BxJNEKN7VJkM9xZeE8hjalpfzm3wP3H15LU7ClJDDDgvxKKzusSI1LqgorFaIQzJZd9zaczakbniYpEKjyitoiEPiOVvaBw81TjU5TQMh0JqR0F7ESy0FcI8Q09vlpzCc8NT%2BEIjU%2FLVta81FIxfV7yMM10licr5ISny5%2Ba1elJU4i6Fdk2zOgEP75NadbIDrNSo7FKKNrCeWrAesiToFnQhiOlKXP4NCmx01lcsFJXkNjsbq24iK5VgOjGU1YSwmoVrcYJvXduDbytg2cYMa4NL%2FPvaX376Nk9%2BBD4f9MKXZ2McGOqUB0qQ6Z3nEfhnSRlCmWNgvGBjJq%2FCOOUFrHKpRkBJZFlIL1yfAPeF%2FC4N%2BpsHkPuEZagY7N0snF2fIyYvwz8PuPXprup433N2Lejhz7mllAaH5p2tiRV9nt%2BtYtIqYnOIbZmRK07HNYRvKXhBz3C4VIbnZQViopzV9qLfTyXZjc0XkTnh0oNBOeoeX3pZ8vkccFxHhTEn%2FSAyhvac31pUH04mBtr%2Br&X-Amz-Signature=c86986672dfb1778b0683277c03d5e97b8f95f6b48d082c4a49c2f71bd1f813a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

