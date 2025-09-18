---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666M52M2Q4%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIBYAurxaZ78pPTNmeqbJI5FO3ZUmNR8FNd0M1kFN4WZjAiEAmoKt%2F0i6Rbl7cwJVqQH15M7COZoeGHFi2wsyhcJPrOgqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEDDIP1x5nuqmqe4aCrcAx30llhPOvEloShbdLQxVXf8maBpUkmF52u8tDSWw%2FkN7SFVx0kxfs3FgVmQpwZLMVtPKuX4ZcfW6L%2FycGmr2%2FIq85hNcNdaVW7FS1%2B3bod4ufv4FbC3vwz6bcbZ9xDjf8Lra6wEw7VcbAfonm1Gq6F0SpwNI1vZ9rDCo7%2FSE5YHL%2BWiyHHCTdUwC1zz6sfClqKEq7DUxWde7pktG08lCirHgi8allA2TT%2BUwf29upqZ%2B7ASlQpijKR1SkZXwaPjRewdUIdLTm14vMuJ0VQR2sCOGpKBPiOZN5FKcZxtrWxOkbJb1LDc4y2i6%2F%2B8mHyk%2Fm%2BeWyw0kqH7l%2FT7Ru7JwwUlV54kfTvfi%2B4leMUr2COEZLpbKiB9KDEMySlJC9xmZMr5fSG%2BxPF9mTcN8OrozY3NYDCFea4FH4HL2XoTtbvk61XvhlTKR3mU9FqNx0uPwN7bHti1O4pblrKz2YYsmKaiGMc245cM64n4T%2Bxx32j7eL5gg0WJ01jUw1Jr%2Fwvpas4XBA4264CaZordFf%2BBPg58UiGbBonmIADXwmX1ByltWfzb0JRAYpbkQI2dviRbTidLbusTzhBEaXW4OKacPHOvuOspPw0elTYOHdM7a7hD4kTS4qrbckH1ui%2FyMNPcr8YGOqUBa7rinBfTio2HmgVilJYwXX1ekv4NwcH5HTr7SVTM5MdFbgOWqfp0drF9f8d2dO8qMQbI%2FRjFnvpQQNr6TQU8FQT2hJvAu1Xvfk3vjOyukx2DbmCBMddIvtxA3v1XOg03NIk%2FxDBnblgMqnCK35eAo3INU1pY89fMY7sR36TbfVtZgeYQGJ6ubbxnCPX749p%2Fr52hCpewqpKcwGoCmqmcX%2FIjOgAN&X-Amz-Signature=6be1f98845d9c53064b92a750612d59910b41bdc075c22162b99d6fc5954bd17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

