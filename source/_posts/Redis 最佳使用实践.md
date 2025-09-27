---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7CGMXL4%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCACM0Brxlt7fng%2Fn185Oroxhr%2B%2FBlDjzvlt36w7FxBFQIhAOy0VwPx3JZ3EMp8VVzLcwFF8KvCGm3mwcDfnYsKryLnKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxYT6guqVF7hupMP1Iq3ANiHebzvkrYzDC%2FBw9RQLH4nN4t1mr6M7PDPD5eCQQz2iJaQbFdQO3dO2RDzYKzyKHXgOxABzAU0vyIggk771yoFpdshUm2L%2FfZxfo%2B1wBJprwFSYsnK1YEahdpnyFO2LaKSEkAWATHReP2%2FhiwsUgxk6bk9aXidRty3KTC4si%2B242tjAT9QEAIn%2FxKU2c4liJHHgZPtHY%2BxQyf2WGju7UZKvYBq1FAXEXV99Qr%2Bi%2F4wcoYYQhucLToLeVXp2GEFoGqfbMmHUgfP7GI08WVML%2F0SqWQ0WLiKShff0RZpq3RbPWFkre%2BKnUcEDcUO8pzlGu%2FAa%2BKdQgcc54FqkqtfW%2B1mk7Tx1LG9UoxJl8%2BWq4dtSQSF1cGvkjoWY8SXuuxdFHVMifKIO7Zs4iT%2FaJBJiv3cEqjhJYTTQHjdUQsUd7Sp5cCI4Vy4MyvM4pjC984Opz5KMK%2FJ3%2BFrdgeW%2ByRmN4lJKdLXCIQNRIIzulPvlQKg0s54OaxkOV4e0yVR2LUENNcsIF4AaH5JlwW5b%2B2X%2BRwqg3GJbsJdtEF9QwFLnkUmoOWpyrsj%2BhUGdDxND3zB3tPnYAqeUOXzr%2BQrcj3i9%2BSDMd%2FjXDM%2FpzudGy2GgU6xoCLi8BCLW4ByvIEhzC34t7GBjqkAU96sHdtrK6U19O4TP9cy8%2BpRxxKO51jipnbDg1JD00eB4ZoBLTp55GXQnuvpaD9c%2FCp9R8drbqoQcnx7dtCSDG5BhJyDQ%2BnNuGKmDW5fdJ67aLzkj07wJN5QFeeDbNKOZ%2FH602fKLouIVtVK%2BpyczzEICXEhwKjvA9pM1gG5FQlBqw2mX8IqSZ7jOWADYfAmgKqiEiYlwWcnNbtJfzrVMs7Im8e&X-Amz-Signature=a3a67c2b725ae4456550bd5ac00782971c5eda33ad8cf51662fe9c2a8d38958a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

