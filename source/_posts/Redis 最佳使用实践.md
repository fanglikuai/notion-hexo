---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637LCW6XU%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T080103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCY0zLbp2GH2xBz6wmrUL5f9lYLVR8JnjEswe0HBNyongIgbzpxNjGGQp6DB2p9%2FWFSlkUH62Vcbz9tw7rbmUSIBpMq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDAAwn8hUaRmRZyfONyrcA54DiLLoeD0GufOP8M%2F8moEz%2Bhi5uTaMJZTxLatx19Vnyo%2BlmtcyWB%2BrjHVaCnJ06dqexdbRwxwdOG5MAlkdobz9fsyGg8Y1zHa52qxFpDosVYe%2BLpAhGQR9R91A7Mati6%2F8aXz9ie9BxZKcrGM2n4B1v%2Bm4ODLWlT4kkU1IVU0YCKwPtUYIfu59I%2BxFfFZr6AIO7vFQ%2FN1OKJvV1biZpa2yylvGSw1CLCQDtvNgj7NQn0Rvq7xg%2BjrqjlDNGKrS6Pngxp6cHAcps48KlJBU0vCxMGso301OQ4EbXTT8AFih%2FV2Un04iOMGTceDU19PyK%2FNI%2B1L5fws%2BaCtHSxaF79owQYs1OwlmypfSBSgsTlfQBKa3p9dhqYgXe4CSMzZtyk4g%2BEBCx1f6MVh4NPSlRTwCkKX5OtTEdBg79OPF4%2Bd6UeBW0BtlrkNtfueK4cn%2FhOXxTBafJP9p2fxw9Lpo6EpvmrvInLtxsU2WsiJF1%2BHIfs8RktiENwmcVM9%2FQdWzhYHDL%2BNG%2FyqVNR8cRXCCwIQhexa8GbVfSeiEg9vC%2B3MWSQNs74GZ6Bbj0DFvY81Yir9y%2F0rPsnv0Tq0Igfp0pxmzZXfPNBPUOVD%2BU3FTF04x6SdGK7XTzFJ4nVm4MMOr58cGOqUBugxNovA6DX6T8fHm5Lf2dnfF2oB6sKNR9SjlTig%2FWjc49ndsFcGrvwdfDyT6lnXsj5o4q6MhDeBv00CfuC73urBIIPld9Qvwrcm5A45F3P6z9VFEUA2nXvOyad8BSTWPsBAPYdcYKPzHE9H62iv6nJeAeu3MBAzoOEPzQkKgkQYUB2Pz2uOQXyo7GFUSyULphqyTWfLIowuUElpteuHJzRp96gEo&X-Amz-Signature=ec354be6cb0b789c51480b2acd268dc284958d937d057a0378165b00809c1377&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

