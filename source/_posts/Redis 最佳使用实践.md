---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RDC3MFB%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGUZyMGcDQ6D%2B1XyK%2FbIrbq6FYdtczM1%2BnP7TcF8B07gIgGKvPvcPV%2FUZYWxFlH%2FmNsbOddkJSUWDlfyNbA3Nbs90q%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDKn2pPwwCbhiCL9N4yrcA3v5liUcLNj%2FS2KZyd7Jy6ApCVXRYq5CZLMdsaZXBsivetp3Gm%2B5kfWj2fT57e80ewyuqfBFfCXr8jhE3dnQ4YE6PRU9ebKsPZQF0EzbINIy7q1PjsZvSKag%2FKp1GnDbbCMuG1cO8e9io8Lnf%2Fuf2cwF0oRFx82omo4COTUc2zMtJqsCGW7TkgupcO4MpkAe4jAeIXV91Qlcp5ZUKQGqDP%2FjrZpmUI1EozWi04G6S%2B5egCeD2r4GAwTNKn1RV1APAjLaewrc1unnUXcEK%2BGyMxptipJ4ORB2%2B8gQal1aLfs3n2Dk0nlWSJnKz%2B02b5gtV76vg3YVYmV3BepvxoiurGUR4ACd7r4MkI2joWY2%2FN5ez9jfSBVHTe7cAOvo6mjQ6i5YmY0FH32fQJt%2FRdGxfNtrIMCdwQm1mwaIprUaW02V8IviY0yKtNZTyhm20OciR011IoXLJkvuuqKSKSZloMeVqah9SMJYlNzzOIB6djq9T8zkSDxi2jTWFbt3qDdv5FhtwglaUBE2izn4oohz9L%2FmJFw1uUhys%2B7TNlUY64H4c%2FwdshaBp0b70bXfpDZPllF6uHWlW08KT4lFbMzjL2QUF8BbrLB2RO1X2X79QY2r0yVZcZn7xxCWhs0yMKbx%2FsYGOqUBUb6d3s3YQy9zcxdUFMeRGLmJtScUoqbXkpEInJYz9T6aMToEUDnU1iecPFgPvoTzCCULT87hRMDAOvCNE5xIWMAqayfRj4EjEjJoLl6FAxKSKdjhtPKwOYvoI0478zgGHKVnSaKP3AjByTS9BdyNPWKJXCRcY2E7OgrsumOvhd9WKNVa6YOaCO7xIyTEgaA7wqpx1OhPYed3T%2FHx4YCPGvyd%2FMC%2B&X-Amz-Signature=2e87892484734cb41a0c2117b4b449c102a901d8c3426b154f6d1316f2c3be47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

