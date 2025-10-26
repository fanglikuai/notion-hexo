---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZYKXZBA6%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T140113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICX%2FQE40l83Hb4DktKXZ88oZEZCYRamhZgOEfSdyF1EXAiBVUwVWGpF%2FDqZqScOziSJnjVAeq%2BXTN%2BuYXuUlQ%2FFS0CqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQePeZLaOGky7Bw6FKtwDEaCQnfFzX%2BY%2FmHdDBVkDHZeG8FCvh5s8ph4B7lZXVGKVr1LryTalE9mlMsoS5mkEzHqvgVPQcbOeBeKvHMBRyQ%2FX4ldLqzF25l8pKt%2Btf%2FdI2qWsVzHE6R%2FlJGxtRwizhDp7hPGbVKIT%2FkhImFyaix7Mft5cdT62aH%2FZ74gDgVj2DAijJWDdMqe4vZIFf%2FiahKgmaZUH6cILfTtOv7PYEZov7%2B4rDOABze4VpbqjIaOzNyQG9EvIS0HYcBPFHP%2F%2FQQdJD2xA3%2BKNxysbTcb5SAnTxbxc5ZgdGAeOEOjKamSbwJMIIb7AWxEcFDftngKeybdKK%2BvO2o2698RGnm4joXbvKVYOtHTpF%2FYsidqdM%2FP5%2F2iL8Rl0nZ%2BZt3P3J%2FwBNBx%2BmOy29x6OBVaqRUHGwiimGIIP%2BWIIsJqg5HshRXU3Ghl4Q9hJ2J5g5x55BKDZjY3HG%2F8wGctXHeZfdQuVhSYRh1sAh0slUJrEzt4A8dcBOjEgUledOzR9dTAyYMwYjPMWYGaMuCbrMnmZtSWEoZi%2FMywRgSvDQCw%2FVvX8etLOOWJTR4UUH93wbWLLzssbyYWQUUBpOv9JlsnLnnijy2s8iTJ3C1FCeSXDLEyBJTsEHUjtQOw4M9OXHNgww5v4xwY6pgHYp3wKWxs8ZWcdwMGqtwoixfGVQy6Rt5emVukwvgt93gBJ3HTI6KVf60z89pbSURfXygu%2BK1Oo%2FjN6D%2Fhjv0OfsgpDITACTU1AUMFznyxFXrSejIIbwj46yE4y3rPELu8pUSk9VQG3dE2XtFOBCO89SNd5EEwB%2BRhUuC1RDcevV6QRoHv4NLItiuP7IMPYp80sXzlE40ZiWexud0xC5nNnm0ePQY4l&X-Amz-Signature=eeba5690ba077c4a999b9c4089a89270f71dbf932bf49958dda6d4b2598eb03e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

