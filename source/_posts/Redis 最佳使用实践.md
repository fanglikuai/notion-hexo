---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZW3FTOX%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCICcdogTzK1PD%2FUqpSEbe0gEs6UEUfyqU6opetner9j4VAiAEJqnmwSHNgF10nTyPtH9ctI5BorXIJUNe9xnKWyFckir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMWdkuM1bU74JzxKdZKtwDcRHyLx9tfGI33jsf5m0V2hoP5B2thheQ0YrtAAEzOg2D3JmXH3g5gGPKzH0ufocT%2FjRNU8vX1OHNrpORrkNi%2FuvOxfjUQ8iMH3Tjc6EwPukciXKM%2F%2FEyXeXNmXqDl7hrv5464bPPEvC713dgUOtaTBqQDqg%2BFc%2F4Vd21d6QnwqMxmw2dZdNmWULa7oULuPsDuEW2bdgf2cuIOqt6vZMc42YoJQNEfCd1ICK3K%2F99MKlS1RGmPAWfXDXQ%2BBLaKp%2BFr2Nrq9gP%2B5eHKbTxg6vfjj4YyVRKzIzOhnGeMex6uaUln8%2F2faoCgmXtjfArbygtir8mbf%2F6ivwuIelLQzdzJw18alPZFTRMIPjLhDeRwFacIdsy%2BtJV5Jeby2oJPZ9xJuWPYdoQPU1WAIoTt5W6N8Wt2G48xTWpvXajplV629P5aNKj6U9ayCe8sgqQGaiKMTVVO5m%2BgG70gAdIyt59wPEbSM8At2%2Be8LxQsfBKnQgucXapL54bSNNNdDP9vVENrGM1pumx2VJZFfC8aMO27EktivDFSIk7Cmjkg3H7QoEC9Ii2hv5bzHWVkXSj701%2BJYTtNhj4BVHLD2KIfI4NrQqcQDH7xf2fr9gWnLNgdRpt%2BS9hSO9ieru%2Faocw88KZyAY6pgHBOqiGF%2ByojmEykAy3ceGIYvhh0jMenSCE7lz3kLsXcuzfpRtbJtN9%2FMzIW5mrL%2FE69Bk%2Fih%2FZIdZdSLdtVpZ%2Bg%2FTzGg3TzFCnUbFnFw1M0WmwCP5E%2Bu8v05HX0mRy4e6bdSRcXCbxFFzjz9YLlfHB%2FfpAa1lAl3XGDyqYduGoKNFev%2BGNR6vw6hERObS0GFxZXSgd56CE1bLPsSI7xGLAp2rsUvH9&X-Amz-Signature=b44b1cb4fe89118103d444a04d0cdcc40bfff0363fdaf78fe4a7fe13cd81d28c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

