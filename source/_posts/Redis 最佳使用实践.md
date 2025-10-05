---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HAGBH7H%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDpYciofUUsq3cklprJ68PHmhzn5Q%2FRIs3l%2FZ8drJloWAiBZfgabvuOIDx0OreVWlkzulaHQQeMij76TzYlES8Jh2Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMVzamFRnL0A334MNbKtwDVWs8NAGYRG3FUKMjEja8dPT6ArJqO9VsPh1re1CfzKA8SRuwhuJKbY4cxDMhSZkfikPGs%2FGtRPQH%2Ff9H9MxvRjjVNt9H57dFfSMWDlzFxQOi%2F2m43OByNoGigvxfX%2BlLRC32znRpgTRvOs7sJzPEydx8M%2Fijn%2BWUlHb8LiFZZF34Uls4Xa5KcBAfVXHjp3rQjRqqYvmXxbI8yawfGUrY5wwuPQzMKnzToRtQdqlImNt02s71yONWcrb69CDrymiYbazhRQIAIok4GBgZmJyRyvos3TJ7JN%2FXQvuDlPw7fkvz2GFIQbBd%2BX8mO%2Bs1A3sBZbvdLnyemxlADdb9gmwM7J6hwpEDWiyRRr7l6mlhQae5aH0tkDR98OD3FPRt4By0R2eT6%2BRw%2BzYpw1D9LNtzMlxK6Fn9evVRpl1uUjxCvxPpuIDgf8Zh5wlcqrDYFzMNIL7xa%2BqcyAExhlqTzwvr38rFgCdSd%2BqPtBML0h0JVMc8mxUz%2Bl3INDzMr%2BCw1u%2FPcK4FGoD7gQDJPdBiV4RKOvwZ1H5NAt3aNzV5re12Y6Pre2gEUNB9he%2FJJXfNrpTbmmLiD%2FnuhomwoWwRm2FOcrQIR82UeW%2FvoniL7HGkJW9UxArWH0iyORU73FMwhIKIxwY6pgFbfO2oFBvLXD%2Fv%2BWha5NPPZAj5UFzZEPSSBcPmKF37B4%2FxEyXn%2Bdb%2F9500YGlPoDIf%2BnfAlqNItQ8R5%2B%2FBpVqiL5u9R17hXHXGTkbb3aWwjcE%2Bwk%2B7P9P%2FKBugyxNFeCWISpnT6WnNLtZ7Fsuk6wNuS1Z9qZK1szCo141hJLzOrZCRa1TC9Mr6%2Be21ZuFvHRdmvHZbrZUiAk3bwRIfLo4w%2BHnFNfyx&X-Amz-Signature=b8c5abd0d4131e06e4bf8b4b0c4ca310b79a6b33300df5bb227e50bd9d4e3470&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

