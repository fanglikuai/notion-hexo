---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667N3MWM6U%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC8OCvgSU8YErJiLgQZNEe9%2Fd7wRu1r20nvtU9CSYMr%2FAiEA1HQcpoLsqlbMVca92gVEhiL%2BtdakM8TiGNJKsbzNYdYqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsHTw%2BTIwMNBt7kjyrcA121T%2BZE4Yx%2FcwUvA6B6SC15oIm%2FFx4LThMkTraC4NBf5iiAaRk9i59xG7zdQlKVv3YI9CtVZ4Nr6HrY8zszVgshdMCS8Ela1Rxgw7kQcxgaK80ykFugEUZA3LNTxuELxhIlOLIQ1K%2Bdn6UfaG1xKO2YQpUdA0RIpUZl9NUY1yS%2FdATXAto6uLLmcjo7yHJykys2zKKaJvJfcmDReM6fO5RkT%2BbRr6%2F000IaIZ9vJNkX4k0vmTLJOS%2B9fc0dNUNOXcf%2BHfw2OoUg6Xrhttza%2FSBFcK9P8rg0do8KfZSUB3kgA8R58rjWrov%2FTgmeUbNxIBvi1rRhQmNegRAuJwyY1koCqRvISYKtVh0Blk6mMf%2FKflBeDOdXL5BNYZWXAviskHWmqMiqAwvEgS6X%2Bligr5hnx6mQNcTGRCc3NgwjRbmEw%2FwSy9DuELmlRfaTXT1jWdjaZpD7qyWYmwYh4IcH1i7R2NoqhRJjQR1Kc9oGmp%2FrunvC%2BUedOlOPVBL%2BJVO6BF2O3aHY2CDI0qtOAxU2ZrBZ6QoewybQ5GRUmSLPQu5Q2p49RVluwN5LkbDDcVfJmRYYyHLBbRs8XWWLnQ5Lwk71ACw8hZoFjGMSZomTYTqpj83XdgV%2BubhkNWeFMLTE48gGOqUBfUZoHRYOCkSaMUwbczVdgMZlCZNrq%2FoPSRvFRDCG1UCmIGnhf7KThXGxtj1rI9ataA1WMgng1HkMVoEyN%2FszCK6IBnrvomT0igiMTg9pdVH5fjdcdh2Xsb3fslnWllBrIW75gGxTLDVOW3fEKvLDX0RpELuqFpSC%2FmhAnMvvd%2F41arQ4wypzu2UX13R1Zp0no9M1BQkt450fZ62j%2BdJ107OU0CuM&X-Amz-Signature=933653f5cc2ea543362cf7574485c4dfcb852e990d395d6bc8085da24487b35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

