---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BLTKDTU%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICkJFv5IdhHQBSLCNWCrnhaXHwv6K6PCWhen0ziDgyX1AiAPKS7ZpgiUfXKsE75JGfpq9VYSdVpS0nWJ7bS%2FwzBKCSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSON%2F%2F90JzEls5dxlKtwDfWIcVSjwC5y9LJHWSJAkUblWoMc3eHHXhaBiV58iyU%2BZjRQDI66GEVO2IdyNaCO84U2jxk%2F9hxMapNS56BjrGgcGuSJfjL3%2FapaGgX4qRlWz%2BJlJKwGZNcrtFsH4PQxlMFf2PtMt1yfZVQWhEgebZUtGTdx6X99BekCOjZt%2Fc3R%2B2vcBrqcM4VSPONVOyeJGvwAwLq1PjVJ35Ia31PlM5EiuxYHpCsmTmw56Y%2FN6nrv1reIa96MBxm6R2Dno2omGbrkhITqLxwjUK9YZede52Gzz3S758%2ByXBEZ9wLcCuTSlYkk0R99TPMDELfvYcOUqeWReYfHHcxzTlkXlvgrbgOp%2BalZQx3t%2FcLu%2F9Vuo%2B7nf8wF8ei0lwmTihYZoL7HOaPf4T6CGgKj%2Bfpc2RdPmtuMYoCexxUe1bZ6nBHBdZuHTtAiK3vJ8QjNRHUQEYuVqNhu95NawKZgrVymnoNL9ofXXlJbjOLqK2YjUYzQ8Df80nKstF41zyiQ1sNRSx11yhsjd5qP9nfvSbuDxZ5AJyF93GdfEUQmtPKGbBq%2BVmWIEcajGNQ%2BEkKrr1sOfZDS4%2F8wkKMizN683iR%2FrKAVAbpYq%2B0ZsjAoo30NNmDXO5v72DzocNn5JeUhsX20wmq%2BOxwY6pgHfofBHlZdVjuIwPuajKZL8rDIZ6y4l14J9cqdcwZT6s65sHywj7ZFFA1Wf1HZnaQCAD6PqbED60loz5V3xjVPDICufVtwOHJVzwDhYDBZRBv9YMUL56qSLjc%2FIQ1SOBKwSceaHKM6xTyG%2F06wCtxvUEDq3m6%2BUlKF3kXZi1t4RdQyPPLF6hsJ2EuHJbvCxsUS1YQOuDGk6tFKyX%2BK3aDrURApMXn8j&X-Amz-Signature=40c1f62ea9cd1260dc430c0b1c0f613acc4ed88067df1464af0ddd6eecb060f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

