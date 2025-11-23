---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XSAMFORD%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQCscccTv0PNHtRJsQ0fks88RVQ3hlw2olhrgbxKGWIHEAIhAPjntWvIumRf43dj2GdNFaBxpiJegaoynsgg%2FKaUj5pVKv8DCDEQABoMNjM3NDIzMTgzODA1IgzjKRU6K8b3zW4KdDoq3AO58K207u%2F%2FAfLLcciIt9cqVHsHoZb%2BD7xUbpsi1jd%2F4jgG9SJsPuFa9RzJnzV7xbwHFl9PyDKeKmdVtKDRlD7iB323x7hPiPsDW6Ou3nD0dFqaT%2Bdx%2Bxrch84j4cvNwrE%2BYGMMhWQNCxKrm3fiSYo5Mk6wR0oAGeI5%2FESBRsR5jDNGAuIU6TIfN2oAM5U2BudeoRshAzpTUdOf1GysPhl9BlFCZwQztkrqBUgEJRwN78a7Jr66VPfHa1qBlKYfzc%2F9ELueys%2F7kLUGlgRMI1Y7gJEubEuKl6FCasLJ0Wj8BV4iEjYKtB02cGHcoP5Kup3P0Vipho3dzQklWtL9aTR3NzQ8Sd8pRzx786EMQ29QdqRw7p2dDQpNM7NTizdxMMCSEJ8r7m%2B9cY0zY2FCnK36VRQg7RyRF4lz7cHkokstHqypwAEPUDsNqjeZbkIEKFZ5YJ3G%2Brgd65OFKeHss93OvzV6IZ3iOzoh%2F%2FVaDpUwHv9o4NJ%2F9Dlpnkz8kBlzU5%2BIJDN9PTD0nvLaCtY%2FfVQb%2B5A7HN%2FUEGONyI6dDoZCKBW4aHnspXoXEi1SxmaSWWjDrH9fyqZZ2RItdtMDPMGgKetOxx2tVTaZLoa%2BrFIvax5ru6NykJlqRwrf7zDHn4nJBjqkAah9NYcY%2FMhv4dhJ8T3F4%2Bi8l1ZWQe7%2BrHULgqrBBQyGJMgGZMTTth0MAhZVTOF3%2FbnpSWnMu%2BkMe0HO8ZB%2BxkXahkF6csElOOK5BcIrXKRzh5Y6IG6zfVOWt4%2FK6Bv%2FeVII3WNN6e7UCBwNfQY3fyzW3iYZroEKcL1TqpTVCYSr5Rawe0%2FR7uMkTI%2Fk4dBP3Jqua0E%2BDIJmyrDatF8Z2to0WFj%2B&X-Amz-Signature=8c2b48344966ba97bd706af3f271350c0e7729e975cf0be5734b52bd66829c25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

