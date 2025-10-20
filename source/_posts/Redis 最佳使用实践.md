---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRWQACQN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIESfE9JGoGOgrLpYtME8gVjVSpCSqNmaswFbS8B4MOpsAiEArGvxgi3x9GWaxwBw3h4zXxaKiqn3iH8osc1qE4pk4%2FYqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM3EkFO5a2pOqQRWVCrcA8YqnF9anthRjrTYAQJ8jHNhg4OqkcbK4N2IV7a3bH2t66Mm1DeHkKlkXPDOWUwMMikeAaEilJ2s1%2BTthe2jIOQotNtQ5tMgIpLdICb6Bwa39750bFpLYGpKyGlFnWzWL820eQp%2F%2F4ypRd613w4v%2B9FEsQ11T6pvA81HVd3AFW3IzkfbR6bH6Cq0O5YPVlkmfsQvUckZh5XwyidM3h2QIoqtTxmQxcf%2BHuHSHWjk6hSZwGhQvZ0P8lbt8GMvm74Otk3GmwY5nUNIBO06gKICriu1iRsHo8XSbRehTg%2Fd2IaAasrbtkPpiLAVKsYA0SGpJT6zdey%2BFPVgTQLGMwGr47MpGwvQOueR%2BaWEsm7j7mbItTinSR%2BLDiwXMmkBlfK0GjI%2FM7ebq046uwqtgOwC%2BxpwOYEKkykLCsaQtpqf8koWcpz7p%2FayHpeB34f%2BXjDXCZ1trKBIodcq%2FqahilaDNj39sxSlGDeq%2BbLw8b95O5r6sakGi1GYYlyyB%2F6ikmYb5RiB5X8c74bTjssoYbx3Hexe3tjQA5ByxmMLFlzo0y0VB8PViFfUvSa3duZ8ARiWanjBumfhvyCqtuoTgvZPzshVwGdQoPZPkFaK6ueXwOuzueBSnSared3xbcpQMK%2F31ccGOqUBlXI36RZvt4ny6QkmBY%2BmeizgaVNWUGzC51Ih6qRppy3MEjlYFTDdHPZ33cXFQuVoVXZaakHmV9Gv3KCqvL3kR6rqDWHNvf0K5pX14C%2FBXQlQJ6Sd2UWN%2FUBKHcScd4YRvnMJo6IaHP4L5Du3KKqOVL5kJjSSULNLG3nBRLiK0zDBBbixym764bco7Ay0Zxwo%2BnI5%2FtjmOXXFgaT5gYbvTojol%2B96&X-Amz-Signature=b9f8bf07e81ca8155d81e7ccf57aaa2404ca48637d72925874fa92bea8d96644&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

