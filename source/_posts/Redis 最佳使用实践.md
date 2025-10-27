---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HLPMO3H%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T200036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2BXuAC%2F0s8uyHkkKYjU93Jg73M70FD0BcnbSrCw%2B6VwQIgZhSyUNrn8LQ8u24siqbw0vkGMPbt0WSYhyGZdANPoA4qiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfFgCvm9ODFQMJ1eSrcA%2FGquoeCeeGSr97u5T3ZjhsOelijCcrI98HvRLWKoaSqqjoRHK18If95MCWnD7NFdIwmkYOBzLx0eraiXh089rrLoBhrncG5sE2doATHWdeDvZ%2BQFW1gUWiBspDfp0M0H%2FYzeGLVi6M%2B%2Bw8ueOBq4Sy28mOmSldft%2FdlJATD0XfrRBhthg44aCcL2dNJuaQNOdtq%2FfNE97x0R7OpQrnMiz9Lw5Us4no%2B73E60%2FSTjigiZoxknX5l9VJIOIfcoa9NkP2qNBExgzVAUEFoDu%2BGK0rczd7dB9NHVP80Aj7X%2Fdunm%2BtbIpv8i0i91c1FkydpknBPQLRCttXmTd94l6XjHuwR9xZT5xNSfxNIYX0p4ct1g7tPf3XdfcGEOLbKpmzRZH6EVg5nj5K%2FF5YwM5MLRaCbxPM6WzOkVX2trgt4Rkfksyd%2BZIlbXdvz%2BKixMFXeZ5EKv3QN5gW7PDa6W%2FNlhgOSuysQUBZVC7ZCBEAmsxvcChwGs6sQmmEoOpS%2BX90ClRihXxhcCC19ey5h6xatirHEmbGgCRoSDgOoAZ2aI2ntbYsde5Hzq%2Fki%2FxTLesF%2F2%2BukvabHexBYQlpnEp0zm2btL1y%2BvR%2BEX49A6vu%2F%2FEFe4bq5aBNSkI%2B2NBEQMOKZ%2F8cGOqUByYToi2fiNnvvi2IDU80GJ47dGK0LCdlKQ1HrzS1GGXHoXTKQ9%2F1i4kOSkpk9mqCjhl8WYFs9KZeG5IZKOzw3Fc306YDByIQqS3jxWv27I6R4ahoTuh%2Bo5INQm3jidCXuWIHX6R66cOmyQCvAUGE9pGHzGXLRetK97axY0wJMzcKrB3Kxox%2BN3bQeVR4GeoI26AXa1t6UhR6z0m6BxGtwM1QMa6op&X-Amz-Signature=e1f87a344274644b83cdd81e050c62aa824dc82f2d8742ae90f330c10f43af88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

