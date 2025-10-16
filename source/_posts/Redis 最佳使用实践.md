---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRZT2VFH%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAF0dHXcfCi%2B80EaFvE7jAYnTpOznSd1xgraglD5D%2FdNAiEAgETjmqAn6NIWKrrdWYt7v6Wj3kaaSqpzO0H6iYSTQJAqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNpwGojI%2FKmAsBONAircA1KQqxqRHXu%2BSnD4S%2FHThKhxL187JmXIAIKp9vAN4PfvuBB0BrIlmi8Ul%2FGgBvoD5nJTL9c0G3%2F1bCXQOl2R5780sowSYHsYIbQjRY6X6JNl57B8TQHPr2tgMkkbWRMDbTS3CI9F7fJAcR5zsgy1OxNvxRDK6QQKDLFhNvJajSyYlxxgZNwj9bmyIwQV3ILIpR%2FV3YE%2F8nH2N650iFHa3LrexdaYNUbSBHUoDfaQ7cdEeQHP5t5jnO95EcoYeYIyNMIVngrXNk0YmdTBt6XBHUFpCSZOJ8EKP8wrihVlaJCKRJ2tF6kVF2ODGMJQ6Pmsuevb7YZHli%2BtiDCGUTiqFdsEIvmu%2BJqc7i%2BZqNGlbdmvHR6zmFlVA2W73hraUFe%2FO%2BcYj8z4UeztYTL%2BknWxdUx6vNnBYUlcOH1iNcPkeZHI%2FPzYi7iDmUh1zG%2F4S4QbAp0Ty50Bf01VqKBfvh7j1R%2FsXugDzD6qvCQRCHbD%2FFLEHY4%2FPoslAFbFHezHbKjtXHdV7UPOlPMIvAopPt99D80CkjGahxvoQ92KtGZ4nv%2BA6vASlC7h9JR6UIwbYshOvLwnZjG6z8AhmancFhbpfl3KkbTVRM0FD7nIqBdQdAB%2BKSnBf5yId5J%2FLDgoMPWLxMcGOqUBDLa%2FBrydsP0MexdTa0xoBfQUOSoy7rL37Sf6bPIlmE4sGVgN8oFRca%2FJBoMSMyuROpz%2Fv0y2UxYARWq2DNaUcTAWne%2BBJl30vJ6ASKJXU7zGlVU6PyebWUX5YRU4CvBzOKAUqwrD%2BTtS0CGcwNk%2Fwrstm1AGSdyymGmfwwljGJKegUjiRzT4CvigD0PXHJQkbcJg5wEZ3c3KmNSEs4RJE7XvEd20&X-Amz-Signature=31af9175e15c1072361285522112b5dfedcf9e48dbe80c2ead6c31321a07a0f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

