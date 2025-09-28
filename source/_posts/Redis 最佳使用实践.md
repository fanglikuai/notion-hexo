---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCPPJV4U%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDe8WK0N5Vt3G46RJgYQyhtmoiUL6lrcHEwI1GjHeBTdQIgXtP536Q2aykl6ZRQ5mPKVnQa%2F4%2BCjZNT2M8pCoZD5JQqiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJAj0lpkT01KnEn8byrcA22eAAs%2BBwAzkx1iP2fVJT52QUFXcgqd2p%2BXCMUCivUHflLyMmu28JZXRejA1wC0E8rzpKqe7Q3gvTWGCEmQMhzUtssCWAPRb49UJYRi8fxc9Pj1iKlS37gm2Lhis7vR0qZ4kpchiN0Rm6QPaDLWbywh55mTskgYi7pMUkzjjetAhai%2BRjXa7bs%2FmZZRusJtPmLfkhDZog5obJd1p8wEpzXZEQcduf6TOzElo%2BsE2L7A1HgJVjjA9GtVb51zWmGZQg%2B5owDZ93Bjceo8bqI59sQGsPNgZzko2P1SI9Hixf%2FCVhGyJw5sbulnsucWc6oWshXg2gMziIDDT1741DHIFZTVy4SIruM6tPQmOspiXrd4B6HrjBgkKrSLaP5zI3MBKkO3qI3lssgv0Gq3MF6TzPxc7t3WIWjP1nQufJu1yTOb1yAFRKmQDjctzmEptTWzHwo6fQIlTNFtUUi590ALN%2BqbbpG68tkUa4SQl36o9ZWEGBISfp5u3gJ53ygm%2FpHsQhX448eDrhh23uWpF2udnwziiXSesDtpEmi18w03ycuiVUs3NiUwoeAISvCE8cs2unuc2XPOMj1IjLh7gfgB0U6tn9hZObH6BLkNmEm26iQT3h%2BViqHQn4NE7hEpMNHv5MYGOqUBjLUxgKOZAVS%2BdorlxUVrQUx46hOZVLH08ZbX1LCoR4KBjYsgzmFwJGoAUMsSLwYIOBlAexvAgg3fCPkUNtOtBj9iZfANc1MYVvSxmGBhlNcfRT2nAQA1r6jB%2FG3Zm18BHubsFCdDQaxqMO2sotUvYUKzYn5%2B4K%2FWMgdUJfOlfc8RXmxe15%2Bdlp2Xw1rIyy4XmIKACkgp3YxXzkdl%2FKGbUbG3voOW&X-Amz-Signature=4dd857f5339397d14053822064e32eb6c4f8a39a6517b17655419e45c64cfdce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

