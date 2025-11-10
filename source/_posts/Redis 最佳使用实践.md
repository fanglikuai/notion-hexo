---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM5XL6SG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQCOujSzxtEUyDhsQ6vs5%2BxikXEia3wsejTkOdiNCu9bGAIgauj4dAe31bn2p2xWtohsecbuMwnfwRqTmpyEw0ItHG8q%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDIG4DQ68s9LLFkVUWircA2j2fOP0aQaxGZPU1jUBnTAGH%2F%2FZWoE%2Bt7w2c6vz%2BdG%2BXpt5zS7elTbQ9WZY8jPkmLkDcCh7njoqASNqRN70eV%2B7xj0cRZbimT0BYimUAYdE6JP5fC6v9WZgJWrW25Gsn5Tu241YO3qzH8WR%2FGQVNuD78je4FhW5xPKqXXN9anv21ithkedst48kVUxg7dfXKI%2Buw4eH%2F58%2FkvCMoWd6IL2Afb0s2Bevg5sBmGIIwXfgD15hjv%2FdUL6L%2BVhqaOAFDfrLF1dH%2FlH8OdzzC8Dvivkwizc%2Bv%2Fu%2FksrtErslAnuaOCzFaz3cnFE0grYD06wYLBvt4%2B9CEGagJrWFuIGyOUfFigQqBH12ZiX2659mkWqyMcC3J2M%2F9kiGpJaes0aCri1eodgvunx7NIcXVRbcMPo6nAJ4ow%2B7JY05MaiRphYbCI2SluBisf7U4z0bDq9PxM6cxYN%2FBUtSMpnw6FNhnRtNFAxdFDwlMLhI9UaZVgms2OLpYLcy8ojJ2wylX%2BbCuRvy4Zx5PlXWlV%2Bvy3%2FjI4CbN4G4r0TY7ZBNXB%2BgE3SSbDaGHyO5xqv8ScvwsrFvsFXq0uiL3PqXE8WmtF1ZR8BnXb4Id0wig17Tgy8ha0xzB4U2sCglyP8sUR4pMLGYxsgGOqUBc3lwpWvrnKaBB2a3NAfkZyqoOW4QYbLSfClkB9HaRA0psO3IjOFepxJLlJO9kUey2mEu4ceysmP0t7E6rwDLs4ZaNZjbH%2F%2BCz2mZXCXIE%2FOVDrWWMChgzzISFXvi3m1%2F8Dyp1jdvTlzha9wUSRt4NIuTzRjDt4xnAAPxBWo6SbmxV%2BaVkcpUvdp5%2BFdqbyeFJ%2FbBMqeKswjgRIVDPvmDxbjx%2Fhsk&X-Amz-Signature=42f2f94bd93a93eeddb5effb67efaf298e30a883cf2eb7fe23efec76cb06bb59&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

