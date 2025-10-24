---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLXQ773P%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T200050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAECG4%2BhQ4gbAvxh703qZi%2BXPfNM0x2EkJc3oX6meLbUAiBcFCK5Iw62JGoqIaEPmAbA7nWGJJ2dTHTZgVu4tjfpJCr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMgowRp4FKPVxbZchKKtwDDek7HS00p%2FdKiH0HByISbnBTl3fWtJcLYfFKE48YjkNYjLcuMeVUC%2BpjgmYMcWHau7pNqeqQ6wenXJ8rctmBralXRzUSYdviMfsZdttgW0so6a%2BG9H9uVhFTWBIWSqlqwTzvY5OFH0Ygd3avWlAbZlUHyWcIqwv3AlCh7%2FkVtJ2%2FaFjO19SGky4RmJYg%2FRBu2HZssm9jr7LQFhuFZtQTZ98jgK0j4iPqG2iOa5GP4UxXoKdZxsakU7hZo777PPGC6I7qNOAeCEL%2F23j8Ws70ifzzGc%2BTIhkr%2FLk2EVg%2Bvft5734FVrwRRvrA%2FHi7sAolXG22eFM7hvqU8iIC3piw0ZVTurOWXrL3BpldY%2B%2F%2F49Xc6XGgCNU8MpBScOeIE2nXhigGv2r38QUxbL82ZC2X894fxZoOzq9BZMZffr3jE8D0wx0i%2Buy5c%2Fg12kq43qf2NX2s3x43e12iB6cuZnGsYn%2BOsAxN0BcyvHd5EVHk3r4fogxyoUeblXgaBCGzCyeHxyNuvjeqajNsH2qMqGPv5eBqZPGOWkxbEgSnZj73UYkRp%2FGjHjfbia6wfhWoCdrJRilrvYI2mP085cq4qHWelZcGKKGlHupGfdVqNVcvzCzuUBbLWTQMc%2B39TrEwupDvxwY6pgEDLk%2BSMYNyeO9S4PbYrzycH09R0Pav87jVmxYFRjSP6MPKuXQhl2BTNy%2F93g7d3WH8LRlzOzUM7knt2L8zPAkdAZTID1fhfptemKoS6nYfiMzfqSbHivXs10uzMD2cNIvHzvkCioQI8ZYMddT86DG8jeyXnWZk0VAJU26cMEXWZPbxwTP2owKYXRn7vOcTdYaQJ8aJz9dLOXXsD1n8KKq%2F5%2BjFJnfy&X-Amz-Signature=fdea61d05ecc9bbaf82937507aae2c639c3eb5af7b90fddc7de5e2266f1c2260&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

