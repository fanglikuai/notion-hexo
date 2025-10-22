---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667I64WFPZ%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T200050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIERG89VlIm%2FbUgFHj3trPdngItM821Q%2Bhn1mE%2FmOXEW7AiEA%2BtRIph3iAIFs9cwQf2w3w%2BpHJ5Do7gxKK%2Bk5aV34cIwq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDPesoEnMtWc1RdawXyrcA3O26%2FCOqbET4qEf52aFkydXrIsN2Nykbz%2BR%2BIOcNMBi2TNZzcl0o1irp0GECUnN7zt1GwGm3V4kWAh9sKZT5XO5ep3xCeOPVZGIFNAGomSCI8BYp2iPP0Umjwj47u0JhMkBYM1P5QD4XDQdbdTofc3OnRAH%2BJpVF4eVy6cvzZzLLR5gRRjF01O5Rw%2FU%2BP0UtTilJ0oILYRMMwT15ngh8YYm6nLga%2BpM44IA3BYLrG9pCbCelN2Ro1ZqZ6OkhtA07ghqX5TRAEg8A4G5UwKqiZUnMU%2Bj7B8eocjikyp17kbV5SJhndcBzxuQddZRKWRzJ0VlPwcM2NAv0UUSSTVvnCXi4QNiobuRtzMxGTn7OV%2FM1dcaBe0YaYmEYQnIpGVvwU6WVWU8f6BsuEQXslPlk%2FfmchL1%2BTOPXSBz%2B%2BsaDU%2BYA2iWJ3O1f9dCd5siwZ6LkDavjqYgDqNv%2FFj9ur%2BImzKN5WuZmV3OMpWvN3lQSQSxUyTsiM93rV1J7Bh%2BCezNueAI%2BADbOFeTjk%2Bbfp2jna01%2BbaV1%2BFn9yc8FQRWjB%2F4xNcTibwxyeV3xGVM2nqpfADGyP6OihhcLbA51xoQUteoMJxlebKHv6hBzJ7XrKIXK9DoQ721fz69hljhMJq35McGOqUBjyYnkJukGB26JnnhenvkUj%2B8zNb4538tRBQl5fFoPgTzz0RfkXUChAFIZmRFAJVYrzCQ6jIFRpLb%2FxlgdaDnwgHm%2Bp29mezgMhohLm8uT2%2BOX5HQ4IZJwp%2FZof4ujOnb4uTk2sE%2FVJIWFO%2FEhlKcaN84W1SE6dEV5iXfHmznRXTrDhzGZtDhkMfrxCVT9pyqWlMWt1Uq9sJoG%2FG8DHoj5S5OVouO&X-Amz-Signature=43e918fff6ba77af3af3ff5c0f93bb91eb3ae2d0d44679711fa146447d6d5ff6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

