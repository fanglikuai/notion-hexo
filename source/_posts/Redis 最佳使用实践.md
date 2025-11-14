---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PYMLA6P%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC67Vp04kEW9WeXSMC0n1QMms85%2FI9dbEVu2Tkt7bygfAIgUiJRXtD0t9SZmvDZB5799lMd3oLDDbGnsaoogqICOg8q%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDOFKNaC7quVx%2BV3d%2FSrcA8gOzHUBuEJXr3OV79NyB44c4C%2FEAZ7MdqHDC%2BPSenYpYxwPk2pjrrQnsnVPrARVhc%2Fu7totzvhzRLM3WOGAsgy82M3jo9CdADFOiyfa6rufsLNZTmUdJ07QLrY4p3sw9X41UzXiwYwLs8CQFc9iAn%2F62qUgzuhJbBwpqHX9jORIrrcqzYEPaXaSCbSl2CDCUFwFLXQU7sdjE5zJEE60pN%2BlGbgK7Zd5n2HUVJvXXZ9lB5NaynQagCtqlU49%2F2jWNvS%2B1%2BmUB7b98bHuSKu4Gp1QwXuSqx4DB2n7%2B5SsSRw5HDZRVzcIXCwQ71SnBVcBpL3pYqUt6D3%2B1Vd6BcGh6l6WdNKFhfN8v5BqImVkzVYnfgpp4ueAJ8Vjj3LyrGf70JayHoaxT7iF%2FovUfvS8gebiiwNQaWYVyojTWcWyzBZwwcY5xvJJfifjeHNp6lA0pJDemy7c%2BgbYL8fVhPrm6LUsLnIx%2Bvg4R7%2BIxyrlfHxLwyzmsk8w0m9kE4yCDxF0xOnHp5P3zHIwkkkws8txQW%2FIDVsI7itmUfKmqCZYQNoH8TMM%2FIzL%2BZa%2Bkq3MvOcf2OkOgvkqlh93BusaPvO4BV5G5O1KRNM%2FmM3b2shIlZmC%2FqWm2ZKgDFc7e8hFMI392sgGOqUBNODUjCnyJvE8iBIt9NdhpHoVh7GbzqY85g%2FHhe8x2rUeMpj9Uf7Dl3GpVaxNHt8ck%2BqCRFm6rC3uEXTLrxLRyH8mNCZkN0BC5Tssntyc6UpByWgP0cpHDwiDIQ%2FcoMeQeljbb2MkNv42lB9tML0Uk6v5ovmsyd8vM1LtoVDeX7zqcMCs9uInVbzWnSk5fdws3g6kPOYbyaJCfOEJRqr1gYzXKT9V&X-Amz-Signature=bbef0ac1778988d4d8fcdf41ea99a405a01cfca048fb5d2c14ebd62ca22d1b84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

