---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664XGW6AG%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T130051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCqEwnpU5XeLgXZBvFkEo9VI9P%2Bw%2FTZKZlb7hM7mrYEDQIgSCPyn%2Fayex77r8VLPvmWrhygxgoic788eHg6pNFgt4wqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKDzTivQ2BEPu%2F9bCrcAys2AiU8zCK3Yg4wK%2FOgYW9ssaK%2BtiKd9R2mXlWhMgGiMUuEjgFveAL4NmKlbNHxZ1BWN9gU%2B3HZ%2BWB9Q%2FU9Rfi75WiqUhMdF2hkOvz0NOf4ssAslx5jS87K2BAC6IUdDZXM1yuM9MYZr%2B64X6Q64bVwmQP7N57L6YX9v3qEABd7JJe7WrHCJKEm5fbqxlnlhPNQmS1sookqQkISWoUvHce91GIi2xxGVKmGlrBd%2FlxHxGiiO2h6LyCkAk%2FjZim8PuvZxjSPSh79r9FAuXWlqRYms94YEZoc1Lm8OUSjVzJwpvllusp%2BerqVQln4RLuGi5CVYyPn49nN3jVdNlNuC5YfTsEmJ4BjnvwT7qt6uOeVbiav1W9Eu6IUOEwx5qwzMqwg0zSNwYulTGu2vaj60hAerGW42NXK8DzvYq7SdY%2BiniKR2%2FXCQhy20VfX0CzrVeir5%2BiySdNZX6TFzA5mDmTgYZSN4C%2B09LeGkpToS2eSdlCohxlguRDIXmCEDZUh3sj%2BQ%2FoH%2FfO2Cw7BKP%2FjEDA%2BCMwyYVTXKkvkKJceA3ryE1gpstsbem8rr2vN1uEoaXYciJbGbKLaG2VgVNVYec8RmlKCAWvVtMmhzGjwQtVREco94kJ3IRR7jtCNMJDj3sYGOqUBBF%2FiOo4P8ImXIYDWBwEax9k9u8lKIfL02jwToxwKvi8onBBPGIg%2F%2B0mr5CvHahskjeWpdx0TTKIFxfie7cgbQ%2Bx7VEeUPOKRyCMA87zaZ2ifAgQsq7aCRY%2BFjLsRzDM%2FU9vwiXzIfVIechgBto6Vm9Zo2jLkieAddqoMpN6R0dR1oij59jC4p%2B5aVaTK7%2FNWovjWUnzj4ovxnwOswjkkcX2d%2FrsR&X-Amz-Signature=192109af789383913114e157c5d18efaee85c33c59c2932a0371c2691d8733b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

