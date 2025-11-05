---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ETG4EPW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAXg55j9PbKADkjwz4t4zuxscn2%2FALLJQ6TLK6RJgCwAiB4AwNO3Tlx9zSyo8%2Ba8uNjPN9me2I2cJqK4FU0k4t07CqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMukNMZxEEK5lJsMFrKtwDjg%2FUAHcJuF%2FSA1RJHc4g0VsCfyCEyQUsgW1yvEyd1oKACAdF1kG6EsH%2FHBLxguc4fg2fje46oR6m8pxnz6vu1jo2p8WUs6pQW1MqkAJWo7OlZZ8J2vXu8Aefyj55cqLatFbD9%2BJR5Pr28htUfCrIQOnz6zHU2bO7x0sALYUocK24jDRro6Li6cja9sXQkASEmJsN%2BWINrNufovlqrn%2FWmrhU5K%2Fx7%2F0PMqfvvrOOeP1vddRlFzZJyxc7Sz%2FDMYdEIE%2Fh0r9EA%2FDZtN8HIDc1%2F5yT%2BbUTRZC6z3ipkf5ft0jTiZHaj5jUIPO8LSTK2va%2BJQgY2jFYu9SIw9jVamBDbBK73bJ1N6q0snIuCsDypXBCpKl5McVJLj4%2FqzO969F%2BXqh%2Fe%2BUqXu6JDaSry9819u4a8ds%2Fvn5DcfXA421saaaug8yjKoX7a1VxkJHRnCi6rwZb5%2FqdtQMYMQPVWvX%2FYgo2GRmBoI3Zn%2Fod6PESMGZhoY0IEAXs5y4oe7obOq4pTrHkPUVDsV%2FxkowAm56y4%2FPyhKWVVweWqXrhKQy6OHtygj82pnsIFOjTj%2FesGwesNjbw4VmOjlRhNgwah8irDYY%2BZmlu3AnwAFmzc4eT6R5McJO3UZK8HvkL4dAwi8muyAY6pgE%2BntSoPieDR6%2BSP8GDRTqdHcLLHsB8wozX%2Bv3LwCAhNVz%2BpbCfVVxFNKyLHWR7T1sULR%2F8hDsfidk27SmBxP5wWb8B%2BlyKO7oJz8yu0noaeMfPymLRo7TbjSSngXtL6a3bIHGTgIUadeI0J1uId3T1iy5HcLui%2F4pk1htgszXTYSywHZj%2BCLQtHOlu7JJXAj8%2FJghtv7NwJsqP6Yj8D%2FgwpKGuZLv2&X-Amz-Signature=3351195f536941dae8f8ba02b3ee5556dc426db914601df7c93c4877b97f24bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

