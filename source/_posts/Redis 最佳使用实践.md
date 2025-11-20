---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7HVDN42%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIE4c8r4xyTWHUu6YTP5ZYHkefrHDHa7ZE39B5khIOD7pAiEAn6k5bkn8duf3Hisj5i6DAsnl2H%2FReBHKLuEt8NN%2BIG4qiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCVG6CgB90ITJ1z0WSrcA0OdpDJOFXI9V5QvQ9FHa3SHdHC%2BPFoUarIxX%2BLP0073BoXmY2ZPEkn2nwqCvXC9YlCMfqauy3M33yZmsI%2BSiOTCkPIUdgEVueKnHjGB3tcNbdpBJw7KKw4VUjbEjGKADwakOsxNi8DIWGbpMW7qa02OvmsFIz8%2FVrR0lQLrABVM7qUWmUFV8Uh0b9pfvSxeS1rG%2BDYEBu7uf6%2BUxuaQIH830dtlJttAREdAYrMNYitUeJIj1wtIUaRq%2FXe5PeB4H8z1pa0qGWgTMhvDVPfJu3ZK3xNkfuhx7Fem9D5HXavsKR4WtyQRnPoB6NDURen2Pe076spVSoQ7FZ59dNjZXpGSbTXpyMs%2FC%2FhAayzJvZsVfVdn64AI01NVbveHTUDW7opT2Ef%2B7yKN4qQ54rZTQSjUecW%2F2rmYkIq%2F71s68pxxHUz94i2ASIWkyt5iqmPv7lkgA%2B%2F9DMTrCVNwfZT3Ixkd7H%2Be10FZhe2IsAE%2BFyCLW2DvUP8Hr%2FvoU%2FgiUX7QbGVL%2BEaCXh4qWjEas67FGI0W0W8O%2BofsP455F1t57rMc1fcegdb45PFilhMzWfN9lVi2mIGttTMchWOFtxkPmzYud6IpaEb3tGEu88wYZSq7K3yxHnCUZMVcst1kMKuU%2B8gGOqUB2T1YsDIUh7wrU1zAW4iIIlG50xi7bDVwY%2BUZP59S7f7U51ltVQg5FKI%2FJy3LAD%2Bs%2FVKE3LosEmjEX4RcNpYicMyGkeTLN%2FdmvGhl6MW6r2at7Ka8x7HzfteixN%2Bi9GDwZZM2qJRwgcFfyDLi85BqmOwO%2BLBi13X4GcHQmQ3m3poY1KVZJe1gkBFiclfzrxqfQpCmR2w3cxGAOHkkaUCszGnITcp1&X-Amz-Signature=6d5ab8d53caa2c88f1a826422b2cd04a0aba13745dffe2693f2bbe637431502f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

