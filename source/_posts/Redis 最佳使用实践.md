---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XC6RNV5F%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGdVUFRZh%2FusPUUF7QaTHVfOzB%2FDyNtR4%2BwVDaZZ0wo0AiAtEcdpcsXw%2F4D9D%2BFnJDihIvwMASoPHF7wqlK2B4gJdyqIBAi4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyFmrAcTaNEd%2BModkKtwDkr%2B0XK1HAlumwfvycvvxadThFX3yFo2V2UKIl3kvqdAy8z6yxWQVEUajCix7jETPwyECr2XMOQu5TL92JYI8ddsX3IPW185%2BHlbY9B6BegaGMGAfXR69nNM%2FohFNoZLPw7UGW1xlWfxl9K0Mu%2BFKegFuBosrXlNtFw%2By%2BN1JMBAOvzFQXLSrrEpBQGKreE3ngrYhUuEPGqnUozYrhz3GsnGF%2FMTQam8nFOjBD3FgCSvlnmYOMmZEAD9gUX4al12UyVDuidIjBKVxIWDdj3oN%2BcoYHM7zdHjS%2Bb0JE%2FuGxLEPdX%2Fw2Lkx43zhCKai9NURxbDFzxZHSAOWjedU8s0DGmNmR8XW%2BzV41hiURZAKobqVZAtDbnwxKbmiuHoPdzPZEzhPhGfFI%2FXFNGbQ6TUHVrcqdg8JmH8ArByo9Ra8kBgOHdm3W0sUDXHd%2FHw9lXx8KXgwjlnYttSAD9%2BbwnhZ91XO0AYqdpMivdWvQouxNEZz9nh9c6tunZZUtHtAHM%2FkhxPK0yNL0NIGXHXTGkKtzZlmPSz3BHN5bdNxABFvD5VfYdjOXbxew5IIp7RddfeUV9J3quRF1wI0rGtJ58nIlc2Y45bOuaBkFbk2xqEwBZ3qi%2B6gkghh9PeQHJAwpuXMxwY6pgGqnDW2diYvLKFlFj%2BzGloNFHHF3e3vxx9dqfKwO61cSrJer3ag4l5A5p2ahA3V1OlCnGrHNSZiUXqrYj%2FH7x01KaleYZFJf0EBLaOtgJeWa0O7qpyUqIRViJlt3raF2anQ4i7znulK%2Fgtin2AFunpEZTIEEglorhwrVP%2Fb%2BQZYthJfkA%2FcZReQnVb7XBahHXlG4i394Le54IxHkVvWCWcTIJVbdS5H&X-Amz-Signature=44fd5b11cf196317058f4626b74b4143387e9092f064beb52bbd25d54681ab2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

