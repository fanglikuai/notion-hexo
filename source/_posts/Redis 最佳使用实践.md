---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUG4XCCF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIB4cBUcrJn%2BbpAKyoy80ofy2TKGmSnLq1KPqc71AwgFIAiB6XHgHpNVqTfG1JpIkb9623XRD1BOEzcgwVJUt04KAPiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDTVaYNEtgAWv60qgKtwD07Lhcj42XF%2Bt41WkoT5MJbzPg9zfxFh0buFcSCqFQjj9NQ62GXBNqVSLEDt7HSs%2Bq1PFBniIdFDVA2H95yqB8SSvuCmrRoP0gf56XrhXkRgU8Y3NAjrrKsQHefGlGic2cBHovrkCdFjKA8lh%2FpZI%2B2NnkXHVjSP1lxbHR1nQP7ShPbvUXCgpFieBSD%2BqtKaFzl3A06CffDL7OMzawHFpWUR5%2B%2FdqWhtDd6M8KIWQGhIR1X%2F7k1%2BqGo6AI9t5WJW0D%2BFiKWEQwJ9SVAz652d4ntcuKUDiOg2eOCLPOIHZCABn3iFs6fPXiXS513p3gFK%2BvpSW31aCd7nTVwuf56hUyO81f3klBD2aC%2BIVMuQja4Ae8GhikW8RXt%2FaqqO6mrLJTU3CySixGNFf4367XfaTEfnCSs1A8LCqO3ywmGHkUTLxcl%2B%2FrYj8e2Vo3GATbWRpcLDrgth11JSoz3w31xJ2NK%2F9cjPrESataadPX7BPLHLnpNwTILnrlwVoW%2FqfvLEQcdhvFx9qepUKEm4v01yJY3YWYTL09ZDfG1v73uEQQkeLY4oNpZwK2UB%2BW1W00u64X%2Fx9D48Ljxc6HTuK6zimFov2nEr8tRKQOsTqyXd%2Ft%2B8YkfPHy8i7gQaXb2cw7JHpxgY6pgF17G%2B0D9czaHOtMRSavywcb%2B2aWgRQXFeP0Gfrj2E65mhrFkmLzezAf9%2FxRsDQZ5wRk5zncaQA9SP9Yr46eyluu6BaIzgam%2FZXZ3yIt3ug1T3xXOuvL0Ud%2BeWF0d77z6T%2FlNx8mvYSt7h%2BHkDR%2BPO8VpCLrHb1PBKeb1bUybSEmqK1zq837BO4nwxk53LaryeVv1zqOz51zTUsTOLqqvmv5G5wWeDg&X-Amz-Signature=244f4ee5f870f125e0f41cebb34e0c7a5a20643ed43553cf5253ad7a85234617&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

