---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKG5TXZA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDr2a624LuCI6mpxruWwqi3Z%2F04VVIAwzbabwg%2Frjx4BQIgDunuG8zESg2gT9x4nW3jU6WITO4LLDXF4CmSmt4q3Zsq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDL9N16zzKkk186ayFSrcAxF5iSSoALQhija48FImru%2FlD7AH89i6B%2FZfL5hS20RzT5BPOxUs9NcHzniFh5PBmbYZNuZ0bUvqELid80DSknBcFtJ%2BgSjXf%2Frf%2F%2FngyTNe0T%2Fcw7u%2BLhHilW%2FUHcdiSOtlsXXqZdj5ZuRBlKticCRZ6WjxQEMrWxPWMWT5614MIF44ngDOexPFXpgm8lPFp1tPB2jjKJkXZ5IAytupyR0hPc%2BTsZgWGLTh6TnhuA2haLgHctkuMjx8PocTbe6lQs%2BunR0MNMXk7koMAkpx8WAzQwUIlk%2FPC9iFmBRm3kdWY9Bvb3wT85oGt%2BP8xbFA4lkFGfQk56QZlzqP3RDQ4O3C9BnF8NW0OyKb%2FOdfPHqTnnx9TI%2BNzEO2QpWEFgjyAlxllUCm01288JMkG2oNWCjKgrEsUDlmdrqfPCxVZZkiKeqCq8frHUgVPRMO6KAAbXna6Bt8FOd2W2GmlnwlnP6lbDuKHtK0ZeQ45zS7l7Tz%2F3SoPDc2PjYDglzp5G9E20RAHrrh8geKSeWmv9BfXIdi6smHVEBQTjWsjqRudey1KoNs53in7Sfjziu38%2BDQ8lhQ6TCvGPaoxLbhec4mCkmTyPtDIt0NdGcjjZycGrdLLWpuSFdn%2FSYbAINPMIeEhccGOqUBlZ%2Btiiwq%2BggRhlCxz%2BOgbvPZOf3tdgheDZswsJNON0HhEzkTpblxC9RNsSxLKn4PGKm5iGTvBapXio%2Fs1ooCHdo1BfVKNxFq4rv8tAZqepR699PAZXFp13lC8nt%2B58HLUGqu3GQQgzRq1QXoIX795DTUWGhcQLY%2FISxFYKAFAVWByLGGCdK4UAvcZf01u%2BvTkwXttGSqHWtb0EOxcD8Q42Zoy6Kx&X-Amz-Signature=cac3d515e7f917298d1e5e8a3aad26499e57edc88087ffc2e469c540f44abdef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

