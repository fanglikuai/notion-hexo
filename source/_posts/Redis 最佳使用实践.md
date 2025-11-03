---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UG7XOA6Z%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD66Y2SwFD5Pf7i3ITySyPq6u%2BxFWgvEg%2FHVfYrSvPddwIhANePD2UjiS7z%2FMtB3MEVlhW1XlM1wDeaa240EHmdpiNbKv8DCFgQABoMNjM3NDIzMTgzODA1IgyCGyLWaXMdt6YQcDAq3AP2Ngoh%2F78WC57jC3Ei%2FNYNTIF0QZ6nDcUSXwLuIe2e3waw%2Fhn1B27kGJW7yCiFXMXppxcN0Gw42150jYzHnIG4Fjj1WPMW4C6uxpp4CclUsEPJgMOetjoODQpccxdAG759DpTE7H%2FL%2BD6rhCKtgUt5a1GQz50BOb%2F0QNThb4EdcFqvlcwlpTjV3S%2FJqYCtUUDbjSlF2BeC6FMR9VH5d2QQwUtyucNS797u0X7GSOZnlVHLZmEvgBRwSUYi55Fzk%2BHq7jPNH1c0FMBhgFIrT%2FdhYc9bqW5RPoIQ6qWcNXC1yWCbPVdcm7BUehWsI5rBDqKzU1%2BpcKYx5H8D5r%2FpooKjscXLEAqZMKA3%2Fzgn%2FKuF%2B1gai1yXwDqguScYDCksVVVr7ITSebmn%2BEOVDDTfvWcIucoS1N9NMgAWEa4GqT%2FX1%2BZD8RA0iLzbIGvl8foaOiLc%2BMH3HUeNzunM4XnUtXx7HWoDkbSqG%2FxsS9qz8Cguj3Pzar%2F6Jxree5FMLkjs3Z6BLSG%2FD%2BokJEuZv29NmLAj1tcCuxUQ5XE7xyFRS3PG5TqUuyR29HIlKnhX3XQJO5znnFCdruXyJH%2BcMAacfXFlVrpYwAWTv7FlUX75Ny%2BG2liaesWHCSXAiCmG0zD4rqHIBjqkAeL7FlOiz331iEikSRj8JGJ9wa5HYysWP8MczDIBQW0BYcgZ1aDl69O2QmDaLiG2m60fNcg7pm%2FSYfS6tq1ng5j2CP2Ke4YODXuze1FBPCkZ7aOKztzwfchLj7NKTqlVBRxFnyfTmpKB4KGUvo45G5dZRYVU2iUyYiGjk0AzlC9H3zC7qDIV9K8CH37Ogg1l0XFyUrv613wCb9O%2FZ72vOpZUKYr2&X-Amz-Signature=a33440f25d0032fb78fe62b97e9ea47949c00f9ae81cc21f51ec3fbefdb3cdac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

