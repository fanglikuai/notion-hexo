---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LQHG2MT%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIB8CWHCGCI69Q%2Bzz0WYAQXHtKaL6yFyZsJCEf%2F8GGemKAiEA%2Bb2booxqFsq%2BERexx0kLEeMI2hoH3IE7wndF9e9uWfUqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBwCp2hRnCkI8AEhjCrcA87R6WYyxAKPs6kAAgwndSLq0%2BToP4WQggU5%2BR3wygAFiNoZ24TCHMW1xaChOzkrsil5l4565FNlzdpwKSNQYOxJGE1xd7NLEW7h%2Fb3BdJZp18Eu4y2QTeHNLKMch%2BXx%2FTcoELda3%2FMbu3c%2FvBgw44Ic%2BesczllJ4yXUaGAy97%2F9QOQWO7OMtxJbamrdmaE%2Bi0GoVWBi%2BSSslnNESDCXbdW5pqOBu4TnUWUUiAFdxn41tucuzYuvTNbBUegyT7k4rq9tIk2xHYEiijK0RC3uhqf8mDn%2F%2Fin6nZssM4Zrsj3yUSYokevRBLr0Lrx0cg24Q7IpaL7uzm6mZCdW5YftVXv6uZoWgm5a0Mb8cHwVuMhvTuUWSaWDtoc3NrS5XVzsZJtf3A3WUaP3re%2F53o86AOzQXEfIgli7Ujk91PiQSn07Vpkzyrum6a%2BhphYDvLX0FltVfmDTVwgdLiY5gmp7HW4CxYQ9LM9ev8oFpSFXbeUQl4rAArhRWOtMX9H3xbDR4sGhf4jsAft3ap7T24KlJGRkDTHk4uxIjEoiJaVQdQMu4OOIrr5%2FefGdrsInAUj2B6QwjgimY8C94z7YIzXSWNUDc1k%2BMwURgdSX7nRHrW36hRsiDhuZKXtoZlmVMIPj3sYGOqUBSBg46bNE2TeJOLLA%2B6V8qUe9p7a8YBacjkt3d5u439dfSBAVJN8GDoNHF%2FTcP2XXXKV6NKkkO8G%2FHAf7Un58hY2tvPNFiiEuEy8UgEhqCA%2BiS69Sw2eHwNj%2BDl5kX3A6KGvasDQG%2Bs80MYa03NuhgQZk6uBGySgbt5QP0W187K6yW4JxmEJqt5bGwRAyUzDziLXiE5ASW8heUtIyf0la7lk%2FjEKh&X-Amz-Signature=2054fad81f6359bbbf746aa2d11d5f0616d1be0c379406300df8b74e7d60652f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

