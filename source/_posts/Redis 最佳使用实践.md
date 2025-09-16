---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFYUMYAX%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJHMEUCIBBO%2FFv3zMk77UHdzkRZeW6M56xe%2FAatPCKZsOnSUbE0AiEA75fDY9k5T%2FeaQ77a6sDrHJAyQXgxpAlV9Yqzljm3IJcqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDALCzlI0MsHvNWKtkSrcA623FaqXZzG5PS2zDBoYyyS9DHrPwh4ZcOT8vm0TU5cDYWNwGUu04v0rIqsPmsu8QXJhxUhyjaX0HrZD6vNiafJokM5KPjXfZFsD26TbMIXACtLbyl4naixlJA5MSDRxYRWLYObfBN5aPl0zSH7Ly%2BVsl5clkuPsF2s5LEgRFLuYy4BJ3pOqDm3M%2Bau%2BgxikqsKvfWSoQM4mQl90ct6w8P9zerqmjXJj0OOsbevkZQ2b27AowqbjdkOuMA4jsiRMMgEAUR4ifv4k%2BmC7ycwcJa0a7DTxGWELQADActXfGHkF1pT44Na4hK79ijBgvXzKKce4rorhHTEqhuoyws9d88V5EgfijSVKGxk7PZLW8Yj3lOOeSdNPOUyvcFcUDjUJFyi%2BntIkabjv5V%2FSYlgG0SzHtiPQzBAfj%2BlT1Ik1HQFL1aLQhjvcH5FDvQeGUaV8T3s4gcJ1xfuCUU36c8ZDRMK%2FDZvFHj0xhXP6XeWblsABIRlieDeOrnrVkXPKkKAfW2e5tuko2tLtgWH3Y8HB%2BOuoK6ml7XR73LZr1z77PzP2H8ppWvGyT0U20r26golcntYh6yQNmZFM%2BKKZ2xoRmjUircV3peC%2BxNhqq0snJBPdVzTBp9ChgXLmFzfDMP2Cp8YGOqUBgF6qu%2FHKBEmuR1Q8IqKGxt3k7vwXa80ca1mlkBVjs2hV66O4Eyb1ysRuUjjvE9R0DWnOXutdbMuqCg8E0ZXiz%2BEdYJ34G0qyC9tWpmww2mT78jLOvJBgETmVJ52yHDJcNJgJRz6%2Bsibs39q09BT8VUjo1zO3hmW4HbmPJdvnVScHX%2FWmz7pxW%2F4pmJSSE21TtFS66snvW0ZDrhtKqwR%2Bovah9MU1&X-Amz-Signature=fa88d046b4dcc113debdc8ef528d456660bd7e6a3db5d9463665cd4117dc4109&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

