---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YVS2WFD%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIFqmPENWcT3fkxQ8Qt9oK3Wqh0Ky%2BoMMksSUgvK7Mlu1AiEAygJOqP%2BFVH1bAoLmWN%2FHFqFbcreANEzw%2F2pxBpVy0Hwq%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDCWOUhLUGCku9YGT5CrcAwUD485lnNYBhzAgDbfbI3TMaBwfUNnVwx%2FAh7BR5J%2FnM3W0tJfoVDegFke1yGZ6A9RwovWIi5HoR%2BXJr2Dn0glB3bvpV7pxtFzPmhF7hJekqIUpsW3efz%2FseRRNpqzkpruZDwL3MpJimcWTFklJVIghWGRqV1VnLliIsBtrIWxPY2M4C271KbFTZ%2FQfSJhi3tU1F26ndU1vnvldn%2FWQUx3zJByauU6BHwTzKzbhBcvrJZ6ak4mhHDAoFIiQ%2FSm7HyIhuJNmDLb3NsH57i%2F3zXVt4lcvE2yZVz8cwj5ggmKDJ6RF6eMcXTJGMk41d0YC6ytaFHPqfOERZPhqChRlFk81JpbbGP0MpaU2RVnORGPFGDyorBgKTZx%2FNmKwbkqBFlB7ZOYPqeswdGfho7VFHkuyA%2BXAjtAe%2FGT6T5oXmm1%2BzXFLq8Rj3Ifi2xAPICjbBQ8woywOrFxR0HT7xwVam6%2BNI7rplMonROVFa32WnlJRGgQtLVzPcCfNrkhQwwJOs6g9ibCMeGr3gZTYfwUVSOIuadrOy6OBuDX7ib4Ye%2FaH%2BnQxgaYbqP6v2%2FdoTkJHzs4XxbkEcVuONmurDrLmiNTxg5gIMiXcN0E5%2FtyL2a8RXMAIuycSyOV9bN0aMITG%2FsgGOqUBMsSCw7TwiXtVBeK%2BAQZKFbrlnGM6sttXO%2Btz8ZuOSMZRhkicWEpC8oZ1yCEs%2FyqHahNYHh36SBIlWSX5Z29Hzo8K8dM0WLBX%2Fx355GCjIw8eyFPUcwKpu8BTf8b6KLvZTEzZ1jmOfyu3C6PWYCSZ9Y4kEpWDuaX0sEWD81fRASRYw71EpstW9yK6W%2BgFijGbZj9KCedR4W2D87dvTzgwlj8KxKO0&X-Amz-Signature=9aa163d01d657963919ce812774ac8709140f7e475b4963801d97461496fc4da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

