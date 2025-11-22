---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPAAFERV%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIFtGmvv%2FaM6pnNaS5hw%2FUVv5xajRjqlulXmZqnwCCA01AiEAtWAXgmyc2z4VPIgfqP5m%2F9X4oE2uFxKFHcNZ7iUecMYq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDJXq1H%2Bh5w7AhYbJwircA6JCpv6yEx8kWSw1u0CPzVbEEOs98NRMQKh8%2FmI%2B%2F2coBqx7aU8o8pAlvUyVSn0GfK49%2BRGu353U8Vx8MFUNNlyi6SYe7DCO43K0G6IKQMkF2K32a8H26hiSUfTHdU0ty7JuCG4uAVNheKnN9KLaB5wXCGrVkKuWwoNROqLVCSSuutfp9MluMy6Q5Fa%2BiwjU2SRduREMtAw0g9MiOJGJml2YCllKPwGsOWBTh%2BbUgLS%2F%2FbxmyBIdjVnajesgFWAet0AGNn7ckO34ZUVg1O6PGUDqdTTjeIONlu0UhkZclwsO4TcorhMXB2lM%2Fi9euFqhTX4ChNjzR%2FN3K1MaHPQ1EYAyg4dFESkDwktz3%2BD21MtoPcvgaDGZtDebkrhfnNKwJh2ThjR%2BHnoHHIEeLjXz7xfDUjIl3fiVS%2FOrYN%2FUG3XlPhDuEByHNX8Ep1o3KTXqe52s1OIb4fZB21Z5l3nXwGNXT5tq6eSapBAO5%2B%2BEYD0%2F7u%2FN8FlecAOavMNa3cagDag5Ft%2FUTUHbfa4avja7Bo%2B%2BB6lmLHz1UA11L2W4oae8fQryiyYMgeH4Wjhkyhwz51xDoXQPNdgFjTKOW73zvEzGNUJ678hKGPzMji9knJoH34YQrd%2BaFWw5NWB8MKDmhMkGOqUBvqzdL9U7m9vyqnB%2FYo84CN35X5brU%2B%2Fo1O8tzUZi2tGCpSXYdlztK8RZYyjInJMT7gvTpV3YP6CNFG9yq%2FkZ1%2FlfsdjR8nQYLIH3WGVQrNy98vnJfz0rKa0X2OYxs1u6gW28a7V9%2FX%2FXM7WEHfhe9HvmucxB0HqPRZENQxbmwEtqp5Evv51FiM7bznrp%2Fe%2FM%2BQKzWQcWF%2FFI4Rf1CpazWeVPslnm&X-Amz-Signature=34cbd9d970feaf2d06ef257d97290e24ac9f296aa57901357ade33d8c82b3a59&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

