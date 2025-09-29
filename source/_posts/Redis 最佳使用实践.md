---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4AJB4KS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIA0K%2BNffrDUt1n0NJJJUGuUZAUao%2Fhstpo7l4T4eMg4nAiEAyF74rs5sp1XI0etOu6MGjU8IAwxKWhbsRgTXtS%2BwU34qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHABf1DqoApLcg5pSyrcA19kFD2GjvaYKB2PsdP%2Bk9qYiIypSaiE%2BlXH%2FmjarrvFjrGSpnfFy2AAT6HFgn%2Bc4MEgqEEQ9oBW5e3kPLVVAa8HnZ3C%2BOC9s7hlH6PAP%2Fkp0KLsEK0P%2BcsDKUh8s46kAryYso4WpbiqHEIIytQMw47DDD%2B9jACJ09am3RSWCFilyaX1b7fDF6qYcAgqlVCJW%2Fw%2FU5QfqDeq8XfbmmuPrKeSG%2FNTZDqXoAg6gSPNm1%2BwZ3O5MjoCa2Xj%2Bj2E5VbxiIKCP8s90MwY0nqlapFZa0KYtsQgPYbce5273bKUGkxc62LfMz3U2wA9mY2kEPPy5PiplhpsnTjlMy5RkHWa%2FAI%2BWGaGElVWNKcRTa%2F3gPrEMlPvOnQr2j4qp4xWG0LL4N1xkFKfmID4tZIWEFRBwk4jU03wxA6QjLu8E8OQs09UesFzTMn6TjRqampENBd66DsQ0WqRuci%2FBgs8tJaXQW0ZLxyyGD%2BakCySXodbSPEX7eJkfzQTOWceND5F1t4sXoergevStBx2awQzudSd5xWi5tHVXPgmzYeQWPyDOhD8Snu6efkQZwINDETZoJRa5TgkiH3YDBIvdBZLqocpsNsxLXNsjqMXhzPqQzinJcn2r3yNwJ7rUFJbDJh4MJ%2FQ6MYGOqUBv8cnECZvE%2BiLkaTHNY1b3akqlaKRDX7cAAqvkyQc41T6P%2Fkr3S95VpIi%2B2tIyQc8svNUyMHhYe8BcC8M3xlPxI1YAY%2FmTmW3JzEBt2xEpe6Oum99TtQFdtxvSTV7Kr%2F7VBLWAm%2FItDv%2BsBuZi%2F5MHpK%2FGrKzAXmrkXx0SqkjAiUgoEeeFGAQ%2FvM3aBb9YsZSZlRSVgb26%2B8BxswAk2S%2FPvA9Gje1&X-Amz-Signature=56a5f63352435d6fb87bda9efdd481903dffe4794184a8467d97538a245d7af0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

