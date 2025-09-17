---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R4QFUCH%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIGh93wd4EAyu2NZ%2FuzSAsqOBXiLeQVn4NQrVwDmBIHYwAiEAvEkSbIXHbRAESOwPNAGoIyXBqaawXeG2hUhxp20dgiQqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIXpRCG1Xz62nrPdRircA2esSEc3zhq%2F%2BxJpBZU8CQYl3FNFC%2BaXN0Rrwf6WuAJ2qvDLVGUZoKIsjWv5%2FaTle%2B%2FcDBqbCMc%2FThKQ%2BdReMJrss2wpGLI7it%2FmQKU31zQ9Gso3G%2FBYA4CmrWJcR0GjfbuNSJASrzK81IgadyaTtblkhVXTAhFmFf2jLBhKA0wTebHPl9yxf8xwKhHXMi%2FelSsl2UU5%2BmwwPw1%2FeQNWEnqSp1G6FWdVcSujjkIFytsLDrfOkTLrEqq%2BuQn8ahuRSdxP9JQsLleB0rhiwOLsAE2Kxlj0L6OXtfB9I9u698fL02fYPqlEat28noV%2BSeaRIeuCKoXKz%2BIskUl6NWMcDgorXJQgOzCcQFirFNdSseV6jNSt1p%2BE%2B5eU2Jm34111vpl2h%2BniNAYZ1cR3%2F4HA5LwfQ8u9wcUsCMUYQNECKMli%2BHGDxabrFKPPL6OMq%2BNsgBpGeWqaBJO9UrkYyxhY3oHYpi8eRZJxxKx7tsabYA9rzg9I8mKBVlL4FPP7TU6RIkTNEVWzuaMPs2ezKWg4JvBYNdgJngW7yDyiV4LF%2BjyJOdJV4LbgV9XAkYUL4Z9RA6BRELvGCnE%2FyJF7lKp93H9upWUAmSNfHZcuAMrnBcHhBqoVsgE74VkYrPu0MJnZq8YGOqUBfMvhRww0LLsLfWGocs1f64j6jG26g0j1vTriZr7cm8TdIztZzsAepy%2F8ZC0ClHkKDkdA%2Bu3hUqZW3iDk0OSXfd%2BjEBsOlOHY%2Bg%2B2RRPsa9ZB%2B3rcJPO0%2FjIQRuRNyxo7tMqEVvkcvxDWZBV97z8dLueRUkC3t507iA4UjONJyPv9ofP0pa5JBgVCgDfCXWnVT7JfYMQelu2MqUKtpEY7WLpWAF8y&X-Amz-Signature=8150060aaab0a85dea834fae38e78235e1f5059d2c6317939427a050aa0f4e61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

