---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEGIDGXG%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T140101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC5Yeoi4k5V1m844BSJenzT0llBVTMMZjmukZqniObWEQIgHAoCtxGMidnLETBXkBLkBi5E%2BQmwGUV7IpHA7H6Q1kYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAeEHxmwRllSVa%2FZbircA0IoEmM33UMP1pK7c%2BkFXuxR06J4yhXgxc4LeRq%2F46bOzJFEJUbWE0qVV10cgsJ0QI6GqvQbQqRNCVgFIRUS%2FpLfJzok8yuxVjjDNuVTSSeMhp0ctvHZQslv9r1CraF7EkzcR57IHG81LimGoVty8NMqoyiTlvxxmymByzUOqG6nHZDiWRY8eVPPWpQofwHZgVEDx5N1jlyraceyLHAqMwnJ48BoKiBlUqOA4pwlxhUwC8qdHSURZmURP%2BcQQ%2BZik%2BYRvhQxfzLsQVaMLEL29Ykp%2BgQMwX9extYTxaN6rmJcavTrIFTM0GNI0egJLRA1b7R1qnk7N4AjaLmkgY1GpJN2Ijn5Z%2BWixFTO4tOjfF17usaEv%2BhonU1%2BRe%2BzvVrSLTO3OIredU8g0dZ1397o8F%2FNWjJ5A6Mx04feXMsdcvEd3FDMkHDUKinYnB82dWogcwS2CZV4P3BSDgGhj3CobOnKYmly11yuI7oX%2BBxTLrm%2FuxIOQJBzJJkQwuyyDY543RguIOHi35LTh8TRNYedBMKNEc5RELxYLMfy4pCAzXBV6zhyNkSrqSQNXpm2wIUsDZaHz%2FZ3QJVt1tHmlCfvGGySu%2FDV9QFwnrH4Ud%2BjVlY8GLHObeFu7lR4utIbMJj2l8gGOqUBGeh54rigMc7wkMoDUgA5QFka1BgpLrKV64JVJ55hZ2VgiNqfG1T7FeDjGFcKxdo53Hr1ir0ut1aBWIvG1AdBMBWJPSkYB4bO%2Bdox9J7sDFsE5lFC62ziOaUSrEB5uIxPhLpJVwn1HRhOmoo8QcomYsnt95sSGxQJQ9ZDj5hjggQZ9UTm6PLnI%2FeXV4y1k4y4%2BqE4ek1ySMoMJ00%2BuLB8WE3pbdPL&X-Amz-Signature=c0385e93914cc3137eb95879f33c795354d0a5796e451d3153a60da608efe897&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

