---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VL7DEJDO%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGLwAu%2BrfTEjqEU%2BSJw57hSzX4CSspAXKf9XZb8%2FSY32AiAx990Nm45SU0%2Bzp3uolSbSq%2FhaGfhs65vhEpCmSAgTVCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaEiCTHUdAnHGT9BnKtwDBYjzC3%2FJLqa4Cxl23ojHzz7U2T3hppVy4NwIcfjtWd4YDi1bqRKo6et6lOddfwksH1UHA9BhILGg3KwJUtzEC%2FQZOPQg9a1J6V5ombACBAsIUl7we13LxbIIGaRrH6uBG3xEqQjUeujA32qBu0rhhPgf6F%2FpdtHKM800%2FRrv6xFrg0E45eC%2BTRiHYF28chnpnibxOz1d6ijycWYdds4plBhAyglm9K8o9ovorItF2jVDcskMfl%2F0eftiYAchOeymLwEI1WkC1CBQmvXkNC29WduKbsQ9egzx9WFzjmnsXYCyevGpgNp3OtXLcGqCS1vcoqdUHLj1vyVUXc6Z%2FsK4wURU5twK%2Fyc0eVpz4GuUHCsd%2FLsPINPVWbZ6pXSfHADR1wNZZmpKD7IOjaou08G8KCMNvufz0yiehon0KL8oSvdvGMuJ2EbiuxlzuwQRGImypMqoowx9WXU1lZteH5jqMI6lY4clOmZy3QLPCTbOxSCK66G%2FEzoWJB34viqhG%2F%2FvQM%2B71WvZRB4eo8J%2FatqyrjSDjRJe97osAlXS1UDHq4M86KKicX07fIoz1Sz3YWVR1fZaAZ99gnbNgLRXqHuEBvSgJvV6Scxlc7y%2FoC4j7yjPu6cIvcllP0h4Q8Uwl7%2BNxwY6pgHuc3uID2YOgyzlfCcASi1APXoSPYMCULH6blpCGShc%2BaV4mOrZpSO0O2vg51%2B9MSJCtM0lczp6l0BXPrDzBFP6iYiUDWBKh4emcKaQZNvoQ%2BRBSZdOr9Sg4SeFVo14Abkld9EbxMCAvA3eaxMnitz9XJMOtDUGQEAEWBv%2F%2FLx551G5wCF2%2BHHswFPLnQMgnm8WkVwQ6D0qPM1D7%2FrK02BPL4oGQ3gU&X-Amz-Signature=2560400a08a7aa2f33ee3bfbe985d07fb1542738784723a56617219f56c647a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

