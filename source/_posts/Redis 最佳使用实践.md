---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675XC55U6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T030114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIAYppzUCUPlwa9idFGdoi9oUUryUmh0bC8SMA801oOVEAiBtsg1dhbiyXBXL%2BExIRR5cvSqtZAYpQGy1jgLk5lYWGyqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjtWg%2Ba4oFBSXky6iKtwDIF%2F4aB3S8rzHiSNFaYmE86ruB4YfzerYaRGR1Bl6AM%2BsYPfgR4iBQphljWMxiA0HJm5a%2FxAZRUUjr4MqPuFdqWor1giR2QMwbUudKvKea4QoIIDR9q2QbWBucxexm9yUJ8E%2F%2B2EIrOYgwqqj44hyphHAPCILAH6GcaQgk3D2xX1QuRg305kD6PwVFVkGkr82G6uaDG%2FJ%2BUyX0L8YxdV05eXb8Vkcy%2BX0bMSJQoRqL1qvr13YR40BhEK1Omwa%2Fj1rH0dolmnu%2F3XcibMQxsvj5twmoj8iOVqmXxxGcHmnpYwho9ppU2OXrZ52Cs5W3h4FhfpyDGBMCPMYOwonJxernVj8NTK8FaYYot9VLN8V8d6nK5g23aHDci2omxhC1PlfQ7X8aUZn%2BFFYqsBLtbYz6C0VcEqvJFO18qX9IvquPboP7IsDpeduTEoh12p1CIxShDOCbO7HhW5iUsUXvZTWs3I7WvAbAhscJO6DOZTbiO%2BaXYhdnyFCx2GoKUnt4vLQxfzuS9xwMFaeE1RP935g0MYH9mG%2FPCKwVeEuGjKpy9zAQM2umuRJy2KKHONjhGcfxNyWDkwVbY3Q7W2CjCckphK4wXn7sggUAGacVeOgeTg9E3%2F6oKUIt0rigLUwhqicxwY6pgGCrXh0T1qgXOsCqxirQ8Bq8AjB9CGylormeiprZ%2B5CeiDVfKq7%2Bo393GXow5V7jM5dXkXMWSBb7PoHbwq5uj8aFz9CSZucJT3DvvU3JiKbVw%2FoJ34sCUbffwmIcgZqDUKWTU3cC5I2whoLZ6Ti0meMXFfuYjLeOYUrc7tDT7xvKtTDjgIiuajG5kwKaNah7Yr3U%2Bmoyv1C5azIhmUmR2%2Bso2p3i1%2BQ&X-Amz-Signature=fb515b83520020e49fbe86327c7e5ec5236b5cbde32ee6f8c023a745e9325743&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

