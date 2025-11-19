---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSA4765Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIQDr%2B2etowqdAADi43paJUoSO9L8gK2mwin9923N%2BMyWhQIgHvUtbp%2BYSeJRGNJtJgmxzsWnZMmSmBIM1t%2BD8VgfEVcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPm0NKDOz9HZtcY2%2BCrcA8JVWxJJzHUPDwXn%2BvD9uuQTcK2cxDZcjtQtZetp%2BpoWVXl5m8Z9i3eHAKPt%2BnuJwKBKzlsOF1HRYYmfokImObGG%2FqQv4NSF6hZ%2Fst53%2BnZpapHrIKEQPsu32KcKcdf5gZIL22SWOnjrNs%2B%2F4okeSn%2FE5sz%2BazUx6Xc4cPcomQZVf8gVcm3S%2BodEBH4WGttAs%2Baq%2F1TtkAYQGaPj9Cs7YsusykuB%2FmA43le1t57bKcuvOFk21bNy9xWBuIoXTgGvBQSTWGH66tIaXu9M6gfa73aTS2GCl6fQQGo%2FqD%2Fdf%2FOASy1vLtWuKTjNMrS5YKjqraKYf0Yx5%2B5yANQXjBU9L3ZWWvAZZ0d7OvNPTK4WVQK14PS95%2BwX76TEIUWWpiSU2SMG%2BabQUDaChGEUqvUzR6bffAsSIjBA%2Fs70qx%2FHthci%2B2%2F2qlMas2hflrZLom0jszpXgtqfoU7D01rw%2BrJZSY%2BZ2EdaMJQhLGqhzulbzd9Fp5jRMO7EziEASpySRIGjag6F65dDdrvwEvoCCT0IUOSk3WL%2B7vzpIE0ow9ImAzxc716XfTybsyUaP0FgFO6O%2FyusR32lbGYvGKTb%2FG7I6QrAoA0BIp2IgTJ4GoFjWZm30pau7An5REmJ3xi6MLzz9sgGOqUBxOoamyyL3vgWvE9%2BJGzfBYlf%2FGbcP6xomGynzitlb9Kxn7meH2PB4HY4Sr37ihi05jRAfPP0kdT5e%2BV%2FT%2BXkouoDo2AdRFzrQdN6iq0t6Q9vM9IxoCjJ8%2B7pjXDTYuGgJ2j9PpqZGFwYVZC%2F3S%2ByOCN0cN8CEjEIxTR6JWRUbAU7D%2BPKrV7974Hiq2MUwsOCHsN9SD%2FPY5tUB0d7UFXs3VAJcK2%2F&X-Amz-Signature=f0af04118a702ff9e5195a4589c1928a859dbee6aa4190d74dc7b1e86ef58d0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

