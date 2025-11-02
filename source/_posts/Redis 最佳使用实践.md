---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUQV47VV%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIGEEkFtZv1fmkyev3LWU6V4r%2FdmAabXDRUP8nW3U4nyHAiBDgGO19zwaLOSBCeNSUDHyv%2BuiIqIo5SBqv6Daf1KNEyr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMv2o2HOlh33G0qAXFKtwDK6tiClopbl4cr%2BbnydhcUdQ2C%2F1vr8QFowhCx3xp2QjdVV%2B7RZpH69hBB99l61Mzu42oNWa63W3l%2BgGJ7bByU2SsaN2nehmWpm8bgxACDqPQnAj5fNEtM4alJNjzK9NZNNXNVq%2Fi2Krfah9Q5vsDfX7vOW%2FUFCrPmhMX%2B6nMLQGDUi5fwhhNLQHMavtzBvKPSjf3lViwM9NuUFasckU9dJMlkbX2kDF0LpiVLn%2FHiQ6f6lPVYyGsq4N%2FWYfzZ6T5KpKyUgABnSXXhoWC17HZSkIlIJEPseLLPro6eZtnjrpZZuDuNsQpFdY1r4plxaYs7Iyw%2BewAQSeup3GppKI6%2FInFJJiUgD67%2FHp%2FnJD4MSLB733YHgVVhcfuWE35utIvboJyzMhKC9HM2tMDm2u4aaC9zUlidsjVrhZR4InaMwCM0niNj51T7%2BRmJ1mKhvzlk6fAWu%2FcJVu%2BeWcQjULffSaQkTkpjRJW7%2F60W11SUibfdL%2Fe5j%2BuYVMq7BkQMhsikr3OFuKmZckLm8gh3aeD96H9EcndiD1WtvMwq6NyCqJ1bXiuY5BMHiBVBg%2BNom4QCwuLt6n4rSE0JXdfT3hhp5B2PoIYx1dkFZjvacdZj5%2Fh7zT8JCW4phW0OEgw%2BtSbyAY6pgE2RSahduQo%2BvSKCZSeewPiikaq7oEX0hoCixDjGrD4JBKPwXiGoyhI1RGAIAGAp6jTiuHm2jWV2Rpa93i2%2BDYpq38K%2FQO3kwgplqqM3zALl%2B7GKXPIixUZMdyxDJ9EjGInWW3zYtAS42f9JxnwelY3BtukLzmTRocrhlnwJ3PEUkt9EBgMBTH36bMdLzWLccL0w7m4I6NoYSiBOEAfvfc%2Fob%2FjhT2s&X-Amz-Signature=b74bdca37aa7c8a182fa6c293c5089205dbd80f88d07e2923bb77911ecb3fec8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

