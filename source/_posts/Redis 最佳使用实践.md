---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSWQ5O56%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsmHmYDsWfszG9FaaA1IC1Nt0dXZ%2FhnPWqSoMNhXDaTwIhAI6ednnCFXWwQeuXW10lqWslg%2BtpzvMlnZQ5aWc3xxFJKv8DCGEQABoMNjM3NDIzMTgzODA1IgzPJd3gc4WvGcYA43Eq3AMeJypH7yWLinYDIOhhOgHERHwxnzTY%2F2xRdyvMaS0Zmrg52g6b4SunQDCHLdnm7N6tRcaxaWQkcA5aQdTPwM8U91OzvIH13MEJNjb%2F3ubDTE7yyztyWwSUkBm3%2BvibBJV%2BlRPS%2Bh5thXScuP%2BzsH0esOX3VcvSS6utHRTmDVjkva8WuTHjbCYGUEKOa8pBdd09xGUpmUp6i1cRJMPXqTl3%2FY4qXzikyapwCQKQ5Of9pAQnSU5tZi5zcWVEyIciUxr0kX4mFUdhAaYa%2Bh52f7fZtCMAK1yik1HCqykw5MgpiN8kI02CNnDeU2VVbtcpQYy6sUl8%2BLBPb5NqJqKWFL6955%2Bd6SxaltxgYeeEwWRuxagi%2FY%2BC0MSRBGayLo%2BOeoUXJPZ0hKU8O8SZ7B2vte24roY9vgTkKnApK%2BkCvJsj4QhEEF729qti6NZbpS5l6X1yCNnR0KRZdnMwI5VhfuGoYsqAmWO2ZWVDPsysn8InrY4H6jhsqjMY78EDoLmBUrkGOc4XHWBolN4UNk2mTytT4xgFQf%2Bj2JT4qrGAD%2BE2J5tbxUgDj0ohnFp74GprNOPgwjaBbfDLF25yRndXmMMRGMk70BUpOQxzmqltjV99BZhJiT78mQLvnvcycTC4kIXHBjqkASPa0URqAJv0Xgy%2FjOOzBA4VFFh%2BIbf3tpplv4zAjsH82Kl0%2FsBElm1caN87IEtb60FLxC5F%2FKrOCvygUlL%2FoCsH9DLJF2xlRs%2BOeF26OuDoGgyXDdoFfxJGbgNhUSnvqx44lKNH56ER82YNGhQ5UdRIwe1O%2BIt438FnIq34oeFdmBKmje6%2BTHk%2Fn9JxXm%2FWuul1X%2BH4b29ByHcy%2BbxeKEUGogsA&X-Amz-Signature=c68ac52e3235f5e67bcf64734202218c9dd3d839700a1821c80d20268d48a898&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

