---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YXX4RSS%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIAa359vvNe3%2By4bOPFtz8Qz7lcuyAaiUeR3GDtXpDr0YAiB87H%2BXWVSA2T6AexZqGths1BuhwbBemq64qHV81eXKOSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbWTxzmOEduKgVe9SKtwDR6R1g3GJsjrWfN2X5U%2BWV4QtePVkILBP7O7e3Q8dvFOsachfDDQsAsd%2BM1hbC7yJhdh6DVujK6ar8bAn6mU7Jl%2FAHQ5es5Y15v3kWzY%2BGdx0oFZZYx8itgl6B6ooLdr0V4SpM96m8pDLPtZ3nb1STN6GS2dLisGkU2u0R0UK0%2BXkBy4IyZHve9Ge0ScnuokhrboqGRjBIs%2FV6iQ389mmDxEug5MnK8zOk%2BmAu1RCjooVIRq74rCsUKLJKwbmgutrV0GQsx3WFHnhBMVW9qsatVUumrDisDJ6xk1p%2Bpw6AR74JIIArh%2F%2BqZ%2F3ds5eyPOzLmHHfQfsdds3oF%2F0Fcd5VJGw37wFrGc4DAH%2FQFjD31bq8zrfOvy7k1zFdFX%2BHQXXwyV%2F42OJWblcUesff1ABQ5gAUI9X6Rb%2FYm6FnarUjpOzDl8QQsKhPO1F4q6CEhZwcjyj3wzM%2FxuiDMPbsrGad6DEcrRvkqY38mtQzSjOCKFLnanHZeO3t1eWdEtV6Mlrp8itrtKvr70uH59MA02R99smI5lWrbdPyJe8gvYluFGBl%2Bjx8UdAIH1APmLCMn3o24zv9m2aNM%2Bozfi34buer2c07d48q0swJ8McMW13ta0g00tvXUe82fdrOdswoZyOyAY6pgH2rvu9M0CfHJacNTZAiHF1S0lLENhZxeY7fz%2BuQORltY8cnQ9Z6n8zuTM%2BNpmzL8qHyRvynUsxNqkm%2BirTE5q%2Fkf3yP8vWi5CSNenZYNs37bfd3FFsU9gOJ8YYh8MfiJ3AHjJloF1PFY1Kh2ZZr69Vuf92XjfBvyM9GftXMSkRwTCPvEvG5xOhdg2qeR5BOxI4mWVp1sJ7y8EuAD7JFTkD7y18%2Btmq&X-Amz-Signature=4d1f24c9a40f22e8b3c4ed40d84a0f96dd76321640d5107220cf4b514652df0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

