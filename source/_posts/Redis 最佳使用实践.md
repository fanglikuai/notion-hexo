---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKTR7F6W%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHAaCXVzLXdlc3QtMiJIMEYCIQCK1F8i7WPteYiDI6H%2FaIzYN58lf4DIQHFueLv62hTQRAIhAM7IyAFSsMxezgnsUQLEpltc%2Bwlhb49l4M%2BPJU%2Bnf2SYKv8DCDkQABoMNjM3NDIzMTgzODA1IgxG6ASkocpbXHCM8o4q3AMsVX8JuZxuh6C%2FxkL957ONwbfplOsC4mNfxNO3e0f9NyCGoFwp5%2B8J33Je%2BeliRELLw3hmH8XVq18YcMe3VowOMh9rYbGzK6CNjov5UK%2FcJxk62cj4%2F6HgJ1FAdaIwWeVB0qT5slphjKhy68ev1J2XGhxl6bTDbBovewdRP7yuPikGTEaZMbQ3IHM98K6%2FObOFwyyGef5jyet%2FBiDqddswd36W0k38UQ5wUKYU0lu1INAS61drSaKBmvPe2T7tTPamvZx07AvYrWHrnNhq%2FNftCtWMM3UzXCcS8UoTre9OWrftFgi%2BgPbXOSUd0dS4wR6VnVZomC%2FMC0pvWkZ7GOZTOg79YO6SsmHmqJhZLIBOApke32C9vvrun%2BYZ71gl2eCLCLGwvJBYYcdeO0ggaAystd62liTQoCrTPWrPiVsT2sfY2BL5jYQb2enqP4v6x5BIDq7RIQlf4BOTOL6h40ZDi0BDBMwgUla9Jbzm7JBNzabv4Ys6IGwtWdIuF13OGQud9lIKk8UnFaSkaKg3ekEMwrGt9e2ZRiTlfYz3FLf2OhgimoY9ygGb7Jr96emgBwd9sjCuS8TMJYhLsLXLxmvcJLDoUW3foVwH4ilUjFRLoMIJi3xVqkl6UOPKPDCc3NLIBjqkAZ2yGNc17XUo%2F%2FNLRNLQ503leI3S%2Bao1bWKd7a9FEWgyVWO4F6rNj6QxLK2vwdjZ2KbeL5RiVjDprRXV3AOxM61hKoV9xOY9hzKgxAhl3vAo2GyPWoF2EZXyuG9B3%2BFF4QP5s%2FFSW7iYLqc%2FTlr0ab06H7EF7GUZENf%2FN5t0wW%2BpOWWNVAqtgGQfCfFaeMQ8BHDqnY0iwE8CynySrtYxsZgegv4V&X-Amz-Signature=f542e0d34ef41baec55f44d9d6095dd114645a234c501a832080156c5a0b8a37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

