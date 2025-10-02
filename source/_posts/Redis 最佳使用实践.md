---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KBFXAS4%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8PY79fvP4NbZpTsG8belQUaqJqqh9ORKcPGI7GJq2QgIhANQCBwrDf68OnZ%2B%2B1D2j9i60q38k47XOhLLfInjPUp4cKv8DCCsQABoMNjM3NDIzMTgzODA1IgyfhYKJfZld56tCif4q3AO8rWk%2FLSLM9UWSKBMuLK9xvzRElAWv87OfKXzgMmbjGf6Llh2Q%2F1Q%2FIuxDAkYefVLz5CK3jF0sUhNZWtsL8YHp3g%2F432W%2BN%2BjNESezz1vJ6%2BHv0YC6%2FwizDaWz2HMtJY5qwCCfNAzgwsxoaPaFhqZ6uuaqb6KCNbSEqAjvxBRDypM6lStS3rZZhdiq75gerowUV6cJJ5P2cr%2BjCpqhO2XNmqr1Rioz91gzFL1dhqhdKCtyoMCqMvEJV2L1D%2BvcEAFR9uesdDASg76wunnAZoJmDMUO%2BqQcSMWhNcIXhr%2BCIsetSgX3CpJwD4M1aIH7jKl91SPSmSJBfQQbhB8LIwDifRqzg7gdiC4iVUnpkw1GYZCbXOvXWty%2FhGXFt14gumDI82KAw8oKQyG7O3niEE%2BNPVzufe6B%2Bd20RjegtOPy1UV4BHjgfNUZ5%2B73cPZ%2BLeA8bHq15oEJsL9UZUwZBNsK1Z6tslFWCfgNqKLvDBMZ9j9WqaIs2M91Ajk5RhpLCgZLXeJZ4r%2BmNazJ1ilOV1p8zKNfrooy8dxTr1NBPxS%2BFSXXvym1HE3syI7FiChf3lGZAyMkJ7ugOutg%2BHUddUUzwAMegKE1yTpX5hMzTK4ekjXC%2FEBf2ajGnCP6bjDHkPnGBjqkAZbHJ7QrYGhY5zXI9iztCBo9KqHmvPiZQDE4QiDjh4YL4cki85gf31a32cU1yilyg2a4CAU3BkNQLsAZ%2Bh9f0prpAbs3rqDQDwUWlB1YYMBLvBspujK8coVBp7S6ZhcE3WjVg5XoKPKiKfzZtwT9phy0pG%2BIC5pcwr9I%2F%2FpY9Kz6CSco%2Bb0Hh7AmMnA7kaH0kD382O2pM4RyvxCWymxhWxpyOkqS&X-Amz-Signature=9bd1a322ed1d5decf5eeda1db95baf9b4e71c44a8f8f2cac6f3f1c1e67b07a25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

