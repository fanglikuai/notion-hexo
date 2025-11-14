---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TNU5XCON%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCp7dEUAAdq6tU4gb00ihItdng8AF3ZFJ3TjWUBQzsOzwIhANZ7MFL%2FswKin6XfeWjMaQ4IChLkcB9Q63WBdG7H8IyVKv8DCFoQABoMNjM3NDIzMTgzODA1IgxJN6k9%2FsmnR8%2BEmywq3APa4Oz5YeDWXAi0EvK0jcZfL1CnOQs%2FRto%2BsvTRswTjtlDpU1sPyuavY9tVP1YzYMEe0lkVCiUjp%2Bc5GiyCOcoHEl%2FioKmKsX%2BLj3YxkAh1ujjDPwshR775K9uZ3rcpT3aeYFe22S5BHkb1mSMPWGYqaQBTKVo%2FHUSsmgb3oSm9iAx5%2Bj2stTINeHSZC5oDC2f7BkmCxGNy12wi6M5joru1UXKHlYXs26ox2hoBcYPvzSWv5i2PWFs3bdZOva3feljl2AoQMQUqMjywH7fwRurRQET%2BYFd3D2vqcwcDfQApS06VjD9sUTJcX2SwchNcy%2BOvfSutYby2ANrtEsTXhnIaaxBttj1Urzb23aemnOT4jOCGGUZkQcyyBD5465ZAmmuqfANHaHRROIo9yOvfqRn88%2FKGDpWn9lOly8StVfKJqnT8ZQK3TLET8MGf3VIWEuHX3ixVymUbFOy0r%2FndhBuDqnuitgDGPCqb%2B%2FkY91xwLppCfiAe3W3L3D2qwObK9P%2FdFfgNzEXhHmbKTQ2T0UJZWOz7XOwPpZZt%2BtlHQoQlN%2BmBVtGaQq5BCmIe44zZC9MrBh6o9dT5ElPk0kFbFO5l3LO%2FCmUNL4WGyl4lHgSK69OTyakYg0fH7ah77zDn8NnIBjqkAak7GpHN7jE%2B2pz9lTRHhNxGy5WFwimXIjRTX%2F45e9KS%2BJunblVjvRqdklsa9pWBGH2p6y5ujk9suDQVHydTjN8jhRGawoYBvh3Nu4PX28yAlejQh3T2F0DAIEUk3JkemXD8g3sdWJmeyp%2BiFN2mcdmC8bPB8cLJaOQj9NDsiin1KM4izGMO%2BKH7nGDDvmAXjWo0xIVWJymRfFOUbJ%2BSts063GxI&X-Amz-Signature=833ce207dbf9cecf7f0200ced4eb1b651d99fe82ffa0c40dfe6f3b3a9e964599&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

