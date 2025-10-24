---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X4OEJVWD%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW3KgvFCQrzyM4LSplE9J00zkl6g4Z%2Fok1hzFPhSBG0wIhAMvklf7qGjYrnxxnERzBJC3C%2FHQBRP4oBe0YbUFaCZdSKv8DCFkQABoMNjM3NDIzMTgzODA1IgywFlEyVgHMNL7pDkcq3ANr%2FhE0FZyU%2BLU4xt2%2BrkEdo9QGY58HLkN5gTwVVCS2j3aZiUNabZiVe%2FRkrg7eNSusSQBl4wB2PVPnHmrUL%2F0nJIKuyT%2B3wwNjYi92DuAGYV3dIH%2Frt2clmcL6tXASkMOhpxiY%2BjYKtes%2FGoWFAN5B%2Bmhqpeq4iXJ3eBeuxGDF9gU5Y%2FODFhsuTz8OLjYzpCgCY%2FOjRB7ng9ZUPfY8IfA%2BsFIrswf7uHtswkCnngGqKGPpXGpB3QuT26YASo6VFSxzLHyQbAl9sVoGboyZQZvyJ6ZEV7TY4m0voWtbeF0lDPBUIULLt2gZxCAdyVHIGOUKBHqGnsCC6pcMI3o7WhrcktaCQ3PZusKuHd4ZuuldEn4s6ZdaNwq98KsKKMMSXbma4dq6y7JfIJol8SirYnZS%2BulLkbgxf3WC%2FIdAXtUXCB6jDn6yp7Kpd6rhyaDTu9KTBWpUyFjQZ6dsmmOpLiWVriOtc%2FbuMp7bujVW6TmyTge7YJS%2B4cV7esjjyKLVRYPI5s5xfVmrZNYQ3%2B1ia%2BHRXN%2BViPSnZd5fnSMD05vGjkVmaK3s8mE5GwXVlxDfNlumZ1Y2IQKs%2BQh4mS7t%2Fbu6uUxfme3X9rWsdtaiVL6Etm77liLQxbaPZawk3DCJ8OzHBjqkAQ9l3jHxMvbT4FHjK%2FrkNaz57Zec2NgyTL0AqhElW5Wy6Ex5JGWFDepuwgA%2BuquZ7ZMCVrTJY4FQLbKntxD4SJxaky5Q7IRG2tFNuLDxI4wDHC5OyPVJ%2Fo9GDvEmwHTgkWViISYekEOwGPFyEE1zTFiWIKsbaKbY8PGydB0aiRH2ep%2By6vDG86BhLH1PJh0ByAuXkWbiU6Ud7PNJEcLq7JvT97rI&X-Amz-Signature=a0c0225f255baabcf7742c5b54d53447bc74ec29dfbcecb73e17abc49ff225ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

