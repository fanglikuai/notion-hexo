---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN6YZVUA%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJGMEQCIGSv2p9ISY9DT2%2BnWdNX68O0Q4LvhTz63xECUWvF8GbxAiBSEky4TE1PuYwc%2FmU8SaZw1UZL2kQnIBBxu4Ft7ttvASr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIM8VEQZeScQ6SIisXfKtwDCaSqLd4XppS3K51IOj9%2F2lA6cECafQ3hk2%2Fr%2FcMy74dLElLnXhT82uGZ128ivzHnRZ5B97%2BdsWk0yN9PdlrHblIAMPwvbriMdQ4zXCS4TKu5jtuo9WHNhymnypPTbSk0IbTjPXl3qb1B%2BYki5rHBs639pqFybks85J8IWw%2FDKihZd2%2F89CWk2bzaa5mYXiajhZ%2B61FCTg23LukZrJPBYGsJ3jvUQXES4Wtbet7uJSSw0Dh9yjj7FmvferZR7NDr82zAoparzrUKJW5JPu2RmHBA1vlD6YS9QKThW%2FsiqYPuQ63P15r4djMcZwt1LhCfkA%2FCgQY4uuPhW5U79M%2F08C34UOonI%2F8xl6UY5kApWwYpFLJpAG7DaCxmuplbY0YmbI9t14IWrIeB1CKVMhaRn7lUxhWSjOItNxEcJyErUCT9GmD8OInlDIE5Ci1q5rBun7uKW59NsfagvBzbBL0BZWs32V5xQ5FeMRk5Ik%2BsRsjKfOZ3BwsXDggNhQFRGQEprwcM38PsaXGffJFd%2B84V4rNXCT7jq1bRZIGqKDGOOfX5%2Fr%2FGXwZPpU9PDlOtzo3%2Fjo%2BYVRZGx%2FaSI97Lv%2Bom%2BUC9cUlJqJ7lHK6GUtHp3feweFRczMJfFU1lnVE4w88%2BWyAY6pgHiMy6msfECk4v1VZby2JvzY56JtdXCn4H6aizhwc62sE7CwK9Z7lRYVwYpt%2Fpl96VNGTYuS1H2pYCw2xfywE2P0wMhnSnkEJgv4iv7lbSqqgzy1LiiR7Z69jvNaJeSYcHKAdLsaAVO8dAlpXNPDFGwiTSI4YtSOCNq4pCZj09jICWWbVeBn98iuC%2Bsgww6AhP9oSswWICcoMfv%2Ffu7Es6ZIaT%2FhGRW&X-Amz-Signature=1670eeaa19bd2f5f57d7e265ed3bced5c993cf8516f159936f43336264d1d0dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

