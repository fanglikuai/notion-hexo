---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6AZK3QN%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDge5mmOKqses75ygEDuZJ0jmvHhz%2BLufRn0aOv8cG%2BvwIhAKgg%2FpJAh3Ue%2Bk34FQbzkxniDn2h3XGqnuGTPvADK5F9KogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx4idozHZSdzqFkus8q3AOXxtoGiqdsovaluFdDlxJ3%2BpcCkXlEVVK%2BgGvPftlXzmHlLwTaCzc5ENaPXGn4QOr7qgrX8rrv%2F%2BXF3Gq9zClVKsk7L8HV9Ft70osV5aOSET%2Fwuwdk59aFsTCwt3AlYaB3MbH3MJn3%2FB4T3HidhjuDXceQCUJuM%2FqqaLsp0t3WmuUCsxtJC7UE3YKDO8dksqvi1iqV01C4bdLy%2B4Ty9HH6khoUh%2FIPMMncwGKhWQGOjSJR7%2FZxK1Khg%2BI%2FRzjCHwhnXy8mGXILJYy1aLp7N3%2F%2BmO4ANApEIt4YRgVv7pMPpQd%2BiuRmmpsh%2FV1Dz2C7uR1K8qslUd0%2BfbLGlNzeR2GLDp%2BODKk1mugjaZvFJCP8m2OfVE4MsINEJKaOmDCP4A%2BdYJPtDGpIeDvx5QzpBRTvOs2a5cvaDushdW0bgdYMWY6A7yJgHu3%2B5DOQ9WAt95PHSiG18%2B0ZX1%2FpgmrqR0dePBO25ojKFudNe7xMwmEhtuAiz9I8w3ODhuN4g3R1P2fXgaPHuHouDsB4d094QEvlOzfVPu8fGmT1lgM1nKmTe3jWHcQt8D7hge50%2BTesf5dEwUJKI3RiSXPdaXMzTjWILScVobjMUmWRdx6xMRk%2BYCDBu%2FioqUSq%2F0xzFzDUsLfIBjqkAZmRDGo4CiPHqzdkDc3ljDoCdx4bjRKoLgE7mylX2YvVCE2DNBtl65noz7yhHMNOeC8uPxvH3eyvw9M2JxUvVq1RJ3JOFG%2B%2BKZVOAh%2BiYxsRoZVKDYCzJshWu0YmZid4RYrb6gyHDgqIa8iSWT0UP5sHv%2FIrB%2Byz%2F4oCE5FM28IQIqvK7Qw1%2BQAcWk8AuOxEC5r7%2B4ftEXYO%2B4uNLS4DSkGnMWwW&X-Amz-Signature=83b72ca2a70a9a7ab66786fb510519c59874d870a0a249bfbdf62df62c4bf5aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

