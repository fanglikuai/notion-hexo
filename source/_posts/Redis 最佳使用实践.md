---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466333BVHVL%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQCSloj9OxPCwH2oh0K2mLgBIbkveT%2BxhOSPcZ%2FphV5ljwIgHHalf3G656pgGGZNschkR1BrvsaAqu9UrgCOJQi77rgqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFzgqEB70xR4HWfKYCrcA6NmY4jCBzTMQcPIMhp15K4HrykxDq8otq0ZYX9PuyG9oo%2B8Val0CmTSHXMZSOs4m5%2BVomWG%2BF4rrR15mNvwL1hBDKGoz1aVMzNcdw%2FztmB7LUSYklrN9iERzwQJYLBHWU5dvXvV5OLAmBKDs%2F%2B%2F8roeyLpD7d7P9gc9nAXnU0r6OEOoKbCeuz6pMMtrJ0TVsD9cD8mVweUACmr3wzaLLo6%2FBSIsgwOjZZm%2B%2Bm0ogrydkc6Np3YpXZ1kai%2Fq%2BYfl7P9q5JBETY5WAgaENh9dAqQYT1y4FJp1k%2BJ0vmYKwIkWW%2B8KsByjC4dL7vJeppsf2yAlj0vuO9gPKeQKn93Ehn8QJBLvIu40p%2FXFxQ0Dq8dasA0cp9BkODcNktsckFqCePMfIuYVy9K16OsWJM51RpTND0Y8zvEhfkGqeoTS2yrujSNpdESm3yzUmLI7FrB%2Ftup%2BUvq34oKPNQg27ejGs2qMr1EtnOnJ%2Fx0A8yXck1z760sOKX9ruVEIewCaxStFTQ45gufrRWpdNvAAEonvSLBhiWv3aQ61cPohsBsJSO6uaEMmJvJVLWHfE0shZ91Ol7hDtP78Ejuc3ehlUs2LXB2b7RQLuak%2BhrGbv1bmXzV7qZdbw4QHye%2BJ56ErMMiNmccGOqUBgxec8daB0jv9BQWuTPYrCEclY4HpzrRY9wVlRQDmW1Ibkr9Ms5Cj759%2FB5I44p6tNhT0jX0PPISvbYp4qICo7syo1b08muglPFYEwPNTKxm08tLLzs3KWXVtMpMoptWp8sL3ZYlxklNk7pDrJM%2F1shbgOX43d1%2Ba5Tc2NBHI%2FEli3gDUFORyIXbV4BPCP8Q4VeeKOe3Y8SdiUXxKGaT70HyoY3Lo&X-Amz-Signature=579efb41307e68d5fe4f29ff6b0c400b33f959da084023e462befff0d1c9a9a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

