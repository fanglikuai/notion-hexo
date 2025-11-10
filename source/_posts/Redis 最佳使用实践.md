---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZZI2FBJ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDdN%2BWGpRFAx2i8Ct%2BOC8W33ScvfM3pjzxAEKzWU6kZpwIgMuOpQdVJP8Aizci6h%2BbhLU65sGomv2YjUGoHds%2F7Q04q%2FwMIARAAGgw2Mzc0MjMxODM4MDUiDG3C5R52zRvyzOMQsyrcA%2FF2Q3AMsE87%2FW2o4j3UxGJyFLaXKlm%2FIw5%2BvaqCRgshXjMX1lA37jr3ptfMsIOa0tjX%2FSSyR%2FJ0hKsLZfbI6lpHHvfuezgU1dZm0EpibnK%2FLbwPG6hwwEq9%2BLWvQ%2B8p3oPbmN0AXpsXvW9dSL1KPEwR5N5ag7yNHxIP2fQ26ghbRco9sy67TBpcpLngFDyYsIFnJjtzxj5g3VApTvboRV3W7ogoB367KAR%2B%2B0Y%2BKEZ2P1ksQaaJmj3HMFA%2FTAv8xO%2BK3E%2BtRSsH6o3LQP2BbfmyvnIvksIXJ0TFydJgDWIMT9nznyWRXMOyWHC1U9SIt8DNAl5Bsd83LWgZrETijn85pmFP5bI57Wu4DO8agrh6xou1hJ%2FLlzlfCQlKo8sQvxBJO0tM5qCW2IL6dWz4fcw%2BAZ8vQlFcjeepE%2BHYvyNw2eo1TDNd0e3%2BnoHXumTGbxmaSwMbkB6xGBtV2fQFM8uRJY2G9mQLo1FlSeHvE%2BkcK2nFCyechIN1J3zeBEUa4C%2Beni2Iu0i%2FHlDpSzN2eTykLF%2BshcCDvM916a5Pi0qeZoJEq1CvqgHcQoZQuMVCDUbRc4biZr6xKR8ikQt8p5g%2BM0U%2BjLO%2B3UwMkhksKP2KprljJNDlada7LX%2BsMJy3xsgGOqUBcgx1WAS9YtA8TTj6Q%2FOpz64cUJoorU3FbnRPWz6r0mUkHXO42OELrjjEq0924%2BGEbS%2B7rUPmyAfiDUxyQTAuEwE294APvMQi018gB1VOZUUqNkHYIAm19mDbqxOxxpjzmo70K47S8vVoPayruayFkNLLEotWGmqhhrxecINYPz80wXb7wU5apz40ICBT2jNiH0BTCUk4FlJKbHAYV1f12e0mRNWu&X-Amz-Signature=3b0348e34e738cabde0315710058021c2a320cc2027a82d3cee480d3747f0ed8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

