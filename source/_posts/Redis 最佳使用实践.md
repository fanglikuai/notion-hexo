---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FKD4M4V%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJGMEQCIHlowWt8vQTeAvLkS9VNGi6h9VEt5fdNb%2B%2B16D3pnYNaAiA3f%2BJ9tNt5PM4OBkFJCgB6sZAPojClH1uEy16d97haRSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkqcvuC3KQZ%2BIeK5%2BKtwDMjKIL3v81wftvfI6Il4QrtKUDWkrvcrm64nTtCghvo4RDbSsFcTHo53J%2FsUu4jfU0%2BxSVfex1N8ddkHMuiH7CFiVdsZXk4k94LUB9%2BLpGTVKq30dRJhVaKzm7OvTlxJKiWN6Ii6foft%2F7a5GQTOgkH7gh90ni43fWqqQF1dcfnmKLdPR%2BF4vuuBKxN0CFtRsr5xP4r17XMjDq0mSNwdOv3XGnXycv%2Fe4qEtCg15yJnfotrvyXxVsxoC5nyjQidP344sz1nndXEp1qkS1SVwwMFy3ZD7OCenVNlVGrJf9S2K6a6tj4oMr0U9E4UlpMfV8y7rkAq5r9cxF67zw3skTysQUQys8EloyAOn24BUuIvmnbkSr6UUH3DTMowK1pVsj9VBB386i1NGfjk3mRDarbQ3B%2Br3jiswXy24rc6NZYctRR84HrT6bv3W0JtLkq8tmeUFECOJTJacqqoVUxcSvFhhXOqRU59SDKOef84ojBcH8RcpiT3FsdArHi%2BtzRVyfrzkGJjg%2B6bSXqMDnU9l77EO3Lh9hgloYYVnatiEgKs99NMB7zE0VaPAiUHCFNVdvLfMrtEcVqyUNntcj2y571R5ynXHY77MjGl44B0puXCEoBoTM%2BuN0vcoDfrQwg9nYxwY6pgHncS16y1gC6USYUzBUWOFBaur5%2FwS4ypP53qmbbzWEK7BG%2BAwphBPzDO0iZJLEsoDDBzz0tRybdqdR8M%2BSNnFCTkijwCu81NDNhUMK1bKHDJQSIFdXMo7EL50ZRCJ4RMIf7I2H9urLHcs3WxwLt0N9Doz7BIDTlUU3ebeYlDOpILpwXDzTobU92ETKJSVIa6ZSNTH%2FAiKWq931TlYq91X3ezeMkZkw&X-Amz-Signature=f77ba4afbc142b643ae2c45280bbdf95d6219ce003622b62f5d42bba3918297f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

