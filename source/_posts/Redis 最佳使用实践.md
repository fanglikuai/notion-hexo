---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THTAUXPA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T070053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGugFLps13TF8uUh6i%2FeTThRmzoWQ2SH1KRQ3llaVNUAAiB4qDVZIOTlVi53POHalll8aAEk95vtT%2B9d70kighs6liqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaZbznCs%2F8D%2F8E%2FS5KtwDPO8A6%2F0mUx9dv4VYt7giWmpojGWqucmtWY7MnpPnRvi%2BTh4smQCZnTG5wW0OZnuj5nJRq7f9Syf5csjYFIfxdVVP3Ofp75tL6w1ti%2BP6q98lHxzbr%2F57qb2ZLmRFyodIPa0RvN5U1WKTAcHKiulPSnGSRf85nZpgnRoIBKnDTmjmbEP5ojCbPA8Bs3iWHLCISBrCkMI%2FDwt%2BhK9dTbKCzoRyxcZSdTs3VEa6hEXIYhNmUNgg1wL7eXWIofbFRD0%2FJbDyMfy2e4xX0T1jPurisY%2BjOjDEW4AfQ2wb2u%2BjdKdG5qPiSOs%2FAxdZUwR0x6XgdyrU6qlu6GaOGgrNEgi6ie8tn7E%2F4QxgsqJ8VV3KCjOzzEPdCTnRo7wdLp8OcewFesi6DABQ0UWwdQPpDslLQX369qoLML23mw90neLoAJIoM0igRsoDQ3zdnyHbIBw1KE8z9wqbFNew9QdFgkJXv7pPHG107n3HdVRHwmrck9%2BPu7lK07bz6Spjfo0f%2B2VW5pAXPMAZN%2B08y%2FskCRc2q43FYNdhAwTNuCVjUlxeJag5%2FL6CLRbbq0%2BdjuEUc6jpoPvMny0x6bMgWxWTmOE5rIl%2BmKkTUUXHmiNzYboTurlkmViu4bJj10jeVP0w%2FNL1yAY6pgEsSQLn8NFaudWgI3HLBtO%2B%2Ft5lizckd%2FQElGW3a6lIBxli57xqMpg5v2k4YWdhUQROHiZkV4TKKldBN%2FXz2g%2FEM6jH4pL6tPw6svGEvPPuJcQGhLdyV2Gz9YmXDV%2BYp58CI5BdVKoawAGwJjd6Z3F1QCEpbCximG9XANzugiF7kSLWhUDnHe2JgI%2BQ2AJ6dBgv1uQqULOadWlIQ5uZBx6YC%2Bt6aORo&X-Amz-Signature=59d8b10f30379cf13a5e82b94ad58485141a946c11d22ce7232beebd9c76fecd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

