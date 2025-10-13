---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2F3NRXW%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDaeJ3FinEv5EwjcVILmbRixVi5JOsHE2R62pS9VwHqHQIhALdzn6hZmiy%2FfU2Ow%2BtntLaIW8c1SD8WFaKgRv7L11aPKv8DCDwQABoMNjM3NDIzMTgzODA1IgwXdcgjWZuprE0KVsUq3AOjEeCR%2BV9DrVe2nEoGw1bq%2FXoib%2F4qKRdBfntgwg3cZ8bas2eZvL4BODW01Tqe3PKKVT%2Fr0vqVjI%2Fi0ieqSU%2BSvtJyGhXhOs2r47JtEj2jwzoKxQ42g2WAtYRVPzu4tlNetiHHiCb99nH6lM5eWg9IZ4sPlncmHa8h%2BD3T4QrasOLI%2FqMLf55k6NmkH%2BkohBYgHGsmEfT1wMIN4t83ZX%2FdPoxN%2F8f09PgT7PqjUBfRtJOBCta8G%2BCPEMaZK6I0LVHfOdcTYN%2F1XE9Hmeu7cRfcIXX%2FSk5vFLeR%2FpDEtyDxveWX9ozf2h4LqyUKwTMy8NKvBqfjgY0xIqtnt4F4D7hbjo%2BBOtytcHr6Qt1gfWGYf1RijP5BnUdIqXgVSfOqz4HR8Z%2BDZgnasx0LyQ52lOBwHVBOIiydtvtViTX8lrfNgTBI%2FeG1UGz13Dt%2FNT1WRzz%2B4JkgeJWXaXsp1ItliCaNlxl7QUf2jS1WJo2iG9JAHIBzY3E9hBznJyy%2BNoVu5eUnJbMCHjXjYsNj5p5JSPGubN%2Fi4p8pGdC4elzrSFU7dFoKLC7mdw2p5G5F4FoM4IpM5cLhHnemml98XlVMO7LXk%2FQtfkU8PKBM6JLOuXcrUNakT9Xw%2F3dk7kR8PjCg1LHHBjqkAZ4SJnBbpyQ%2FgUDAt6F1pTi3qcHFtXonDCMWeEXSmSKPLzBqCvVLjUkWr9Zimo6BfkdrhcjPoQKeUeMxA2wci3J5KqKHDErUVWHsP9GnRBwu8A5MhSsgCbBl10aMkhPdVTl1Xp204wZ2l8xcUHMpcUcpGufICHg0sWIw7%2Fs%2BkwFpv1iOlDwwqERXbKSEPtrm3ngL3WCk3TLDfkyCO7UyzjCPBVFO&X-Amz-Signature=8355b23e87fd90b69b3061f5353ab6c66d1089efdc050d5dbf565240e63dfa70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

