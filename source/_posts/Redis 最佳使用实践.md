---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKLWWLI5%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBGsc6slF%2FrrJLGoVeczsBQJvupiQhtkJjCN8v3DcEPNAiEAgCR%2BmxdbHGsgMoRO4HCxkA4xREDcdB6NBmbzWkGQ%2FG4qiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABt%2BrydxytPICmZfCrcA60AzxI5anlO2vP866r0JEXded6JqMTc5rpFx3XEmaFfEJm2zGwjpedjm%2B54hbVEkLlIyEVhWUMqB352ADFpadzszkoONcPVjS9mDIH4Fy3CCXs1ZC63E8ybbObSuEZl4%2F8UtzqbeWXdBbmfopN0lA9ktzECukqBnPm0U%2BxQMOiIji%2BBsApKYN2NiC3stE236Hueh5CFFCA3GOAummO5MgvkBF4TUQYGLrGZg6lJ6GlLou4vsaktzynbDqpALTqM0cEIQSs2N%2B9YLx8zcYafgOd39lLEJuFweF9a2F8CZJV8pKwFyutkI6MjVMHtI%2Bj%2BmANYamPcUF0FoJf1hp9OCZMQjWO9G%2Flx6l4O7dmv6VLpFo64YQ%2BUSdO5vPSR5u2E5ucn%2BlacK6kMu2QHd3yefaVLR1SGjqpKfbzwyeOJfKh8Yd8jIJF1gKAq%2Bre1rGfvzDiV%2FpSQvm0ZSEPVG3xz0UalNuza%2BcpSALSB9bUzqhVgVNr6LONuk4NKOX8J0A0QQokSwLoIyfOfmd4iAAdX4vzkXnd2zSxDZVqNMr5m1jUJaIsrZc%2FmoZANwImi4b4W9kJzEWC8a8GoL0l9%2FVCJrBTenqFAYHStXRIUsqn68kyY3CnLWeJb0OVr78NbMM3onckGOqUBkup%2F9Gf6GM41dkEgqt12oHEimcJy7l2ndevyx03gN3vtZ1tt4Ef1uw2xRkXrkTNsTTnhYL2ZsMwPaJRGaGs10qBeK2Od904467p2%2B9z6V6xqzQZvAaHT5AKa7r6INs7xbyIaAHjG52r5KoVolqClezkrI57MCjVoloBt8b78mPhUEbka8I6%2BjNX9ke1dt5fEOVVoJYR72xdJ0JjrXKusagnK0SBj&X-Amz-Signature=e230250b0d3c1e6a03138706b513abff1bbd05765d1e0555fb43c6a855f814ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

