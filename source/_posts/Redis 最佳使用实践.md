---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MT4CVKN%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGl%2FS2ngcyLulVglnA4E2qGj6w3OjFDx%2FhSGLOxDs3l6AiEA9EZIAO74baOF4AmQu%2Fj7AYjM6bbx3K1WB8%2Bsvse%2BTjsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJvlaDYis%2FA6%2BMP8ircA27hY9%2FobQCy%2B%2F0V6Habl9xoY2sAFIJ6gxHf3QjfSXZIfA752GcfzZKoWZtSxkZZ5TwcmnBM5p0m9nLL369WCmYwLJpXbhPR5YlHhqexhJ2grj9DPS0BPLTRMfRVeKnQf5gbZ5W%2FJddjyR6%2Fz0IBQNUgnfkvsH8cKeP5SNMPjDVEXaL8HxSyeaseOPyVBAYLgYhSWOP2mgP8kVcu1Wji%2Fvk5QOSomygDcJkiLhdSjN4rXWkS8WMU0DnZ7Or4wMRzX5lGyhHDucz73ZbKKOG1P3Jtkvk0Kvws7zkJjvkX1XvyGP%2FhV7%2FugpzuZDaz7NhX074E8t0PBJjiSSB6rkIcPU8w4Y%2FNiOYOHP%2FSJBX%2FluSZviX53JpWVPV5Fl2HHG4fl%2FiCiFrWiKkzVqy8Y6jZMwiMds32MAK6rKNmyJvk6eK2cULsTXAZS5aaoCkHJI4%2Bphmfv4mcnTIadxDdGhboZ%2BIcEdKq8AhDdBCi0LA190xr5w1%2BU0OdkxGn%2FFpl4F99IGvksgldKILGrtz%2BUBZXVkR%2F5gRW99cVdUVd1AbrusiyRyaafwwcSC0z0d412OZaALMQAKFz6uWxJhqScGxwh7szx9IEWmfMvCO1rlEfYWtTTG%2FuA8emFmnUeGIUMOnAxscGOqUBaKoHiddeKQYvIZP%2F7pyxuyYGypDgpiZsnEYy2nicSMcrVL0Zjww715Le7NU%2FGxYk3oVqiHMZ6Batez9j%2Bdah1NvCuLrHD6cCf1btCGHbkgn0bjKbxJLUa317ep45P8QPt6%2BprI0yqrmcWFNYMMjqIM9P%2Biz4qFMFP0GKnEUoWVToUZLHIDExKDig%2BMzddMQapgu139lCi8%2FnmbjmsW1Gn5gEpLdL&X-Amz-Signature=ec989943231674fb172cdd81470cd089b24142e57b4d9c27b89b55273bea5155&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

