---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662TWRCK2G%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIEIXXNu0KACcBFq72nIHzNAKCsihD6PBTzEmZZzaLFhxAiAw5ACcs6qfdzTEyyj2Owtq6akxktwUTqqGTopm81w0bCqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZJuoUs0gDNdtVwooKtwDwiVYxKoMaJqZS%2BZohTHgP2IGXV7Kt3jdNoXYk7YESJdZpIpmZ3hqwQU4AGQLDLx5%2B5qAJlOC2mdO5aNsqX00uBDj23YbJbo63%2BtzW3kEbFI8a9LOJQ7llfBYX4TTabuH4rH%2BXfJYsTFqWIinaCspJ%2FBCaDn86rLj%2FloYZzem06Foy36un7O37P%2BsAIyH8Fm1646bs7tHs7zPgnBSvrz9%2BApi37eHAuYprOUpotygB78%2Bp7MJ%2FwLz%2FCU9wY%2FJgyKxkf5TC6ciRDwnMSFBFJ6pd%2Fp30gAtMLAc0KWqgyChewTXMXvz8%2Bxd5m4%2FK%2BFOYvwWckYNgiEDCQyZOhjm6%2FczJtbwE1M4UluT8T5uGlj4v4umuudOyzYTUIiWrp8SNSEtrv3bKfctgkhIAaicT0kbe6jo3z202%2B0%2F2UhlwAigGY7TyeMawN9lGlXx4LpGh7PN%2F%2F3ExsPN43F6lafSiv%2B2JTMXsAsOh81dLf0yoO%2BBRrhCVmjM5bV1lO44X1sPzQDHJt7T%2B9UyIsPqjm5X56QI5qJacAHs1pLd53vIuihmPYv1zOgkeUVCVYUJ4A5Fkx3BtN%2FVwbSj7n%2Bs3o3xqSK9Gh23ENSmZGrfo4wKBCfyfv3etST9qkiUwHzF1Bgw95mtxgY6pgEQg2zcaNFGvHw8RVKBOqQGIEW2n0SDr2XH0aXQ2DwmyZMfnhSvXMxuyUTXi%2FalhbPoxZKtEaFAdIjaD42juDjOXY6D%2BTdHGfIRZZJ%2Bh%2FYmAaUAzzxLUcM%2BHNpM8%2F9WkJ20ntMDTOgWL9jd00uPJxuGreOQJZ4LqMweXzAp%2Fh0bRO6Wcx19Odpd0UwCaZuQUKEBP4T6pVfsZnERrSFe04RR14iL92DD&X-Amz-Signature=137869f8b9527fe26c12b9ea78d608cbd767f9a68842d62426ec7036ae21c454&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

