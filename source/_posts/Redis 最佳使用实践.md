---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FLIQAIK%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQDY0krBwkeajlAMDvWeCHfk7o6%2BTLZzk8Ezft1hiZuV9QIgYxqTrSHSALIbmxxcykdn5D8yrckc3adzw2MaBps423YqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKepHBvCHmp4k88u1SrcA1MQDLhTjdaEPpb6yH%2Bj0r%2FL8f7nRyL%2B0mWG6hvV8TtUJ9ASgLFsKTsxZ37rGPgqVzqrpUBbTGGyKQNmipuAU5EahfKha%2FQfH8kFiITuGtLWD23yo40WSB6ojjrMfX9dDOGPmBZodTztt4U0a0ltqmHK3BLJJrIQulzOEJKjiKbP%2BRAfLstI%2F%2F1AEuR636%2B7EYKfkEtLqo2NHDi%2BCnr6q0P9ZMYkr%2F1MTdX%2B0d5qqazCR7YAtEP%2Fe8IPJDqEFHnTesAvO8rghwe5hW998tkEaqwtMmiE4tr3N2XHmaFXjzZFKVlfdI916Ir3IFmZlFfylA5ZLhdnBmAL9nC%2FtZ2nWHtYrdMyEOfMDBUPACgejuhOQLGXdnJ61HWor4br%2Bh%2FQcV%2BBVjakgbaAjo8LJOxxXAmFH0nFSfuyaxj2DRCqALh8%2B7Exm%2F3TAC6SolojtuWTrBp5aHoidedVXU9cRfQdSF78OYe0Rk0fs8Zk3wLb606B8EfGAdWFArZcEm%2FOcBlXCIRdEzOjHUwZ1VcidvCOn3xI5J7xmqu%2F5dUnHL3%2FfgMhcjfe3eGPCdNYZO93si8BAQ9ZIPzTTz0A90pUaRdq%2FMnTJQlGR%2FMcqwbeMm5HTjlpCl68dRFd3wXYSVrOMOCTwcgGOqUB70Z8HG29IjnhKHv26q2CQyo7Q%2Bl5amfFq1RHww%2Foov%2FZe3eXvhRwL1YGhABy8%2FO6dKt74tTfTiAHc3H94ACWi1mdeYWTguIkawHylCD%2Bz91mKcuaikSdL1A4Jh39pGwIOETZCdguV3SzO2RLBecjmqJxQO0tlT92HRO0eueyXZGcStsAMBjKG601tkF5J%2BA7Lv5EIV6IS1TZ42yNXh7FIJrkYgGR&X-Amz-Signature=19301b4dec5d3e4c15d39ed3be4be83f236e9f99c16655da182f22aae3913a04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

