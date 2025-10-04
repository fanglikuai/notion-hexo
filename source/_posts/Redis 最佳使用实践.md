---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DSSBPJ4%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T190140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDCfey2JBuRZEBwjdS3vZJGSK0dWiSwD4vSgoY8neDI7wIgY2pgbABkdaf7pDQyqXEGOXA9EghTUo6GU5YcRoSBm1Aq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDPxBoY5cI9aO4wc9rSrcA9SMaeqbdAQCJKvXoTiAuSrRnwwBSv2L0O%2BmPk3xHYhUbJoZnvy%2FucfSHTLBakdWEBP%2FrAeDVyd8yTUV2DJxWcCB86e5TeDmgrc%2BRK2sjOdSbCEyHewJ12QGq9SyQVizq6Xe%2B7UF2KU1YST6I724K4brUbA33Gi5UidGuBHHiwxOCQqSPgt7ktX2NPhvSGIbYE9DBhLAGZgIEshKj023KBrKr4T8WEDAFwBddFwGa3KfKPIf1yXZLmrKuU3UBHYpIcC34i3iixDVsrsXGejOjnWm0GSployoQTd3G8XTKh2p1aVfK61eZ7esFhzyX7xtJ8SHr7vVijL5D4gCZcmMDARaXct%2B1wvSaXqoJMhaD3P2tDdkJBuu66kh9pPo2V2qL6TsINWJ886dI6vWjNI2qj7Q0DYVSDSjedpCXmNggCxEjO57uix8KlE1AiY%2ByOuIldqm3OGczt0c7xmU02n4C1990p4NcJa8VzTFM8sFsk5FPvfLoa0AlRDklT982JdpWDy7nQxQqaDmn3Nqdh4prlwvGErogfwpgwJFiB%2Ffkdxi90EWZDkQDThLPGWdvv2MwqHM0LAicwsqOAwlSxhceH4UHvROdg7OYjDdOTT%2FLs6Ji4d5b0n%2BEXZSfKKdMMuPhccGOqUBM2npAw%2Fm3Yi4FOOCkGRC0FMgRogFt4Wad%2BBBb8WGK5NSmhmBFzsSxzCtmri7KwP5BigSbd98Y%2Bn6Q4qIL2vXpTvD6C4ACDi5X8x%2FTYAub5NyDct7P8fUzC7pMHewU5BxsSQdFpje9n6lIXm%2B%2FToa8dTCgM779d40tgGo9QRBPBSw8%2BCDfwVBtEaigj%2BKR4aeaYHtKPrnfrt1JMiVvAwYP%2FK2Adav&X-Amz-Signature=d02e66669ac5bf497f33d34721080288a1132ec2e196287da5da7a91021d865e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

