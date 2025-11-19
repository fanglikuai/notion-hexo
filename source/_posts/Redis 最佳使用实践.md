---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KSBXMOL%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDIYQXtSg%2Bi7I0%2FDdUjzZ5UsY%2BkX3%2Fdxnpv2HSoOP2sggIgM83LpJluZEnVVZO2a2uHVvrXtxF9hlxQgqVHH1%2FWeQwqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOxqhFZLhmOUHmilircA4qbCLVx5Ne5RNDCijyKtI6ybM%2FcIwm0kWb874z7G8ByRdVA75R2lojcqQ5gB8fyp3%2B1jeaDh770em2YOHUExXkAG%2BcV8LMsDiWvh2qHjXqfGYhYeI1RScrOBGofdAn8TAy1OsmbJCC3P19Nj9PPPRd29c%2BLmWmKFDwb6HcQmgDFHjkr8jMwj74Vp9Mz8vtgDOTLd8q%2BFiCIJ892W4LP8j%2FVOzRB%2B%2FIHUJHjQow68fkUN9EX44zTiSYJgTas5i8IhXS%2F5pEy8%2BTXM4PpFgOFqBntg2JBYJL3z6LprmZS8WMMaMY2VhwmfNW6BMbsCLAp8Fh7VYSfzsKUVLdyXSporelGyC4Nc5kdKPd9SOCXDzAP0TlNjL8unmjNKAw8aIQHsZntFmlnUxNO37u5YHQGbLb1hqKWPeuUFU4MUsHahyXgq9WArQVIA%2Bp3Ffcdej%2BcCrAgZMtaxlZY8x0BAQIB1J1Sud3P5fA1uGfKPoM2%2FQZmKng59EaPhNC8lNkpcyUVwVHtP%2Bz%2BKJZkUtoJui4c%2FJyBST3iAAhHbE%2Fd7r0s4P5%2FmDGxImJfeVWyJiDlczS9gqYxF8lh23SLdUi4pX6uohbfz7ApbvZD768VX3i%2B3k6PNRHaFOPQzSKQvnL2MPaV9cgGOqUBsgCVYjKDH2i1FhiMmdKqQXyftKo%2BICQhcc3SffeXzdVAL5WOzusye8SPHEeO%2BUxV4xAByfiRmx%2Bbe8xFCAekakEdJS5jJeMJWRKXOlhjTfFXtUUm2yRDxNX3VrRYZAEyGDzE1ypYG1IzSPUK3MiSbAhvJ349Tabsl8OlVd1PDj0sosmy0dgyosquJHF3I2lSYTUKLpvEZWQaB%2FKgGY2qJxTBa%2BBR&X-Amz-Signature=f549187769a949aafdabd47fb5abfa56f2aec2faf2330d4c5e94b6f4b8e39f3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

