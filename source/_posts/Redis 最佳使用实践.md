---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VK472J2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDnVAxjzQtxe%2Fj%2FJZtFD7U1ncu3ppI3yDvh2p%2FfdTbqkwIhAJ%2F8ZH6eBtYQnyPQJxihUdwB3jhQ2Ikyg99pr3LwrCrIKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwAIatjgLrr%2FmBh0UIq3APlk16DeLqvRUkux63l3EujFbhO4ZsSr5KPWFy6BSB2z0L41H721h2cyMZfJw66glHz2tq0aKHKpS2Q7DKEbLkjrbenKpVtPGzhd5TTVu3CX5T6B6ne7xn4XnmUtRPiI859nd6hXIz7mF%2BdFNE2MZWzn8mAR32ywerhufCkHAmiYVwjSGoXbLsflogKEDaXSPidMIl2bMFKzhHj9hf14KxwM1rLC3QeAXNCMPY2rOPBYeNPxfK2pTKGMoX6x1cPH3l2gLfxi8bP4vn%2FXuQBtrOJzSywuN%2FGRUm13Vfz16Xlrb0YSnbB3F4GfuBrAiNsw0r%2FuE6XYoSSnDu34isOyvbi5F%2FX%2Bh9vT8Oz2P9wFrmyNvfBf4YJFl5ydq6%2B3RP8YcHQdZm0i%2FaNDjEkJr6nzM1eDPrMiVrqi68ePUxatH1sOVKgyk3iACD3lNk1PzB2d120Infg29rtQ91y%2BwdmXaqKRAbd5iDNW%2F21BteabpuczMQTbgkpvBmQqI1c7RQJ27Jr8H%2B0u2wpq%2FcPTG0qZdlbGH3Ztr7QwzGcLHevxJz2gbwEGg56ASDiLvtT%2F47H78sJzN4mEJjlEaPeSeqV5HfOu%2BIBfwyZKM6hDaAm5lmksOzJAX5q90XSZph28TCGhfHIBjqkAWaH29H9pweqjH%2Fih0kynaUvcW%2Fl1vwEDuQqa5GI8IPK4eOPIlvkv1o6mkQbC8WO%2Fzv%2BtnVj3tGR7O0UHV4NS6oh31%2B%2BLYauEqjuiQ4Jzvx5QqH7XiRNyXOysBpuI98U5ZDKvuVEA1TV%2FqHeiLAiNewXiChpodCYGEO9lt2Ixif%2FXig%2Fu9URlBvv5jTL5%2FX9VuTmUgOOX1ORLXecQGMKPKtAUble&X-Amz-Signature=eb7ed1d4a86db4a207d88395873a1e6f2b42d260dacd8717699610378541acbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

