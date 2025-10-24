---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVXRJI4Q%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T080255Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDWKxjJ3DKfjV44mG1cMeIZDLAnxzGwz2F8BXUwQZ1EkgIgF0SzDE%2BATVu8E5K0ak1RaJe6rIwsdsyMVrGviI7Ccv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDGFH%2B%2Bb8maMhvSavdyrcA9WAVsfaW2BEXU51g3E0Uk8G5gVnJvephcQZMG0E8rWSVgd3CRgPJwg5GBcnNSdWdosTXxFkXNSKighkbGQ%2BYvfDOEmUbqvdguIS8aA7W0BXSFWEoQZmZdasWi7V0pff7uqnD1qONMyRRHNFpjfU1FgfPF7sPkoXGHgQQB6HXYxmReJh1sWV70o6W1gfLOfK7cxvv%2FIlR2DVMJyFThUfVeRGivQooG98xlJVBzz0Aw2vXmKDdDaei%2Bzd70qJhgSBAdWX1bJ5km9E3wkdAZZX2F42f9KNZBGtVCYZiOHtCav3IzTp4Drf5FPGSfY8bsf%2BT2s7ZvoLMB%2FZNpBLGG40w6vBu8W8A4hUADOHcwKNeyKFkYrytxT0ogcZdDmwZt%2FFTJX%2Bcm0GzEiynOw8GHZdGAE39q7xmzP0HQOdX1bLLUv%2F3F5QIG7gTbCHri0A7HyjqC6ELo7rJuR7%2FuDru1Ur%2FMoA%2Fg5zF7Xg4AnyGyaYxBee%2B2APmt8lBrJhWCGBYxvkSyRwwKtfyaf2LfQsXGiGRf%2BOwmY4xDir9cYKxyXk4x%2BK%2FhESd9lpDFfM8H5waUNjJnsz0pAddme93esHwMtexVf5yertMvHQ2trtxVRj5KA2kK5s0jboqGtWQ9KLMIzP7McGOqUBd2khd38P8PLfqvtE7zGiuoNRil1R8grx%2FApjg%2FhSndsaz12AmkAZzQF3nLQ5gJ4sYEWXNvtY655GznH7dqfbcdkWSvJyPrE7ev3ICGef2J4f5cQn%2FHSTqgNCFj6jfFEq6GpUV%2Ba1QDXMkCaLTb8ilzNOzvfV1KvP9otX7UrJ3H%2FCpgJE2tXIISqgerg5JQPRpfaVB9KRihJjLszSY05fKlGL%2FuI6&X-Amz-Signature=1fad9f675b3d5e270dfb1b3a68deb958a5f8ebf2ae587ec67ecdf3be8bb361c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

