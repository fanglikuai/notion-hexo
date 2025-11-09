---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653T2LTAU%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIGJwCzzX7bK2lbPExOEgdOtWgXiPD2t4kKRR1ECaps%2FfAiEAr%2BEp9veYktxabdkt56MHsdvnb7kCO4IxXrf6LhXNQGsqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBSGIsfzVL6%2F%2B1DVSyrcA9gc8V7voJ2Z8nE1MmQI%2FKr2us5pvURxqmRHBGDNlUuB3RPnNgRO%2FZXeQKduCMyQNtHFCjYalBPs02R3tuuDNIzwHOcqgGnLUUvwhm3h23EOJsmD3YirrEPMRMhb1MspPvR4uOx7KZ6iDFNhtqwNUfgFZpibw3KVQxrY6D2Tu%2B6Lg6RMb02011oPEXNsrYoGMZeB6g7Qo1L9BTJtMQYWk0Np2XEtkxE4aeXfN1ZGXiQPsahtlPe7M8kQGHMueSzRz8WtO7JxxWrG%2FRzvVC420J1A6tf%2Finf86E38CXiiaKNTWUhgB8em2j8b%2FUUtoYfuu755At7anJixjNaq63dbkVbSIVCCSsf5YedOwZwqmGUgrrS9mQXuB8f6Ocqjbf8IPlDLnUk7OUACfAUWR6ljGCQgiCNb%2BX1dfEw2hhxziixX7X2ndJh9dw%2ByVxYn%2FouC1DWVLEYlsiQAhRqUYyEdFu1J3phFNChZMlksbvuMC%2BwxlO1oKGc3q8vx2Rjk5diT4JI%2BheyHO%2BqBojhqj0mSKRmCH1BWNsXOV2dej3k4%2B%2FAIigSOeUdo4%2F2IHegeR2xpuBDFFw0P93L%2BkYmSZn3lpbT79KERJL56PUbkPSzcYCurfHO8YaZMDEjiDA%2F5MO%2Bmv8gGOqUBg0ZEgmI3KmlTu4PBUetUU9ajhmPluPUFIcEe1olyeb4Z1jvGYPJXNr6qiDuGSJ7xhVSSNplX2Qa7USVEc%2FLNyW72NB8jK63DcSTWzCcP2qzHC0nKJYPk4fReXRvlm04WDR%2Fgfxj%2FpPCyCf7PDJ7LpTsC35zgVsGPogBBafTTsfAHy0oZijHQrGUoXAtlnyNNetkaOLMKVaXA0Bc8LUuX3Tfk8nPy&X-Amz-Signature=9d6f893e17bfac6b3819772aa3585770020584aa49b1ca10f935e71a63301795&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

