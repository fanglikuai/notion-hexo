---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYAJWVYD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDKiYR7CWFBuHGgCs6%2BsxdG65%2FkyYjqHY8u619o3wWPZAiAPCoBdnXQTw3uPW6gHYU61sl9%2FVASsh4HJvgV4mHPnFSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMu656bkVd3UzwfhffKtwDqVVW5Oz1ueFiC4Tkj4HdbARgLL4D6Fd%2F3%2F1n%2BK6RJYo0m%2FGpy89DZgCHegUk2Q9QhzSNpcJq9xHE%2Bvj7rHx0lkbXMUeJaaNW0Cu%2BgIkNUm7zoD0rc6VRh%2FMKPfJM8CxBfILHoagik18KKij8myn2kgm1ZdvrVY2S5oLBRQXPmX6HORtz3NEWmuOUHGJdz2p9zlgrhfadtddwtzHgSTOHu5ts9k%2FcoXKU%2BfT9un1zDSJdoPGA7%2BWHz%2BiEQS45iUrhnSbnPUfoeLw6yjpQ2tKUichjIknl6xf5Ecvp5vQyqKFMiC9frrql4jC1G74AAnDlm%2Bd1ezmFHmrACur98Zld7agOZ%2BhVf9DM6XajS5DyU5xhNiSteYV%2FDQvNe6qzYkJokT2fRNCH49nCdl1rr16vvOxsQsXC3Gwe%2BrrSVq5kBqnRIdR%2F3FVS4tLU5IsMj8WMGLmZAvH3KZFdy%2BTQaeHxl%2FpR4hK6IO5DEFu0PU1CG1exVr0d86hr7BanMc2oIBpWP%2BEX7jxHL%2FLkXXXz2q1TnFzMHMeZ2pDm%2BK%2B%2BoeJiN4D6hZrmVfSUnzm38Xm8E5VqPWScGlzzMEa61XU%2BH%2FA047XEwaLetMF6W%2FYhgnvb1X7FTpPKxLCKu7KRLiowiPLVyAY6pgGVQEfn744PMy%2FIz%2B9hQXHFiJCUVc31khTM3lmozhw9W2qqQJWW6IXqXYBcJqkERxF7UUzjp3ZA1gyzXml51A5qwBPIhgBYx2IadDByZNqo8jKJjaBqzH9Q2jxU46GRQlSFMV%2FS1cFqF7K5eloT7ReNCCWGUOhoT2n6meks6zRsx0cZDZnnYoRayRc%2FGMHoOcGP6TRQE2paX5fnQXB8jUbHF8EKQXZH&X-Amz-Signature=f7112d95800e2ae6feeddf11401c0e7950f5e7f794b4541ee9cc35ca75246059&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

