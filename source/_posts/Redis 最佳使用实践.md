---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IRU6RCC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEYfRW2IE6W5w6drL5Wm7w98%2BOH6qPrNSdEblHU08GEgIgJSKX8IuWVEbZx5ah54p5IShoN04Mu27o3Vk9kEA4XcUq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDIXK5Szpns%2FUDwrtxSrcA9TcyZEe5J6zRDa%2BzDvXFrYlxr6Uw9uKs6Tcdvr68mLJdJJT%2FJBKAE%2BO3Unk4Jpf%2Brp5nn91nfsQlU%2FGnVyZPY%2FFgJwxyb3ElyUhFrR3cl51ULuhMkD6SslGpYJH1tx79AfRIdXc%2BiMSYiJPlbpAydqKab0OP9d71DpEciNzwQw%2BFGIHABFv5iKd%2BZbtU4tlbDEADxbU35m%2FTvC6RbHgh5WGyC1g93T8J9ZKyX4bk9ftbPPSxpAddAPGNFHtOmQg0FJyqAiP8a1tVmIrJ9BoIgr6SyVu9Dm113O83Y9rJS54DD3MLBtpzeZui6mgdECi%2Fmcu9QrtsgwkD5VTejc8xtB3hFRSv5UgXHivCDT7AB7FU2G9tMMw6FVwFXupmAqyuSa6rXhOPyrg%2BqixJnQ13GZbA5%2B0z%2FQO%2FtYkn%2B9BLE%2BOHwRqsMz02Dh%2FbbfQ2ZQhHpB%2BaNjpCn0SAZ4vW3ZinV%2BzUuHrbgInei4gpai2e0LZSkRtFWPO5qEHsZ3z7EOyCduJk71Gz33gRFXPPNE2%2BXwshkKETttLFlBcgUGMVdMJykTeEsRPwADqVA8O3BrRKqcabHCd%2BhR54naeOvZ6nKjXR0x8WX25u8n6oofvtvxYIWPPa41lFsJnyWBoMMKEkMkGOqUBf4BBvHqXe3N1HBFO1Gsc%2FyZnd0wHi6UPDFWKEqBLfma9hW0EtP6ybjQPo13vyQk6uTe41gmQtwjQfxYRqdDO1MBt3QYQSAVaeLsJ3iMzaWFV5vfZW2ehKkCtJjJ%2B65Ww4uz%2FKWZO17Fnxy1kPHIjOna9g7MBC8VInlFzLtovF2F69YUMXMjF0uC1JnsTq%2BwNvgC25QMu1coh0caSq2II%2BCOlvPuD&X-Amz-Signature=436fec59f7bdb1436a6322ddf617ef452d79b7b80e9cd704654e5015f5ab4c73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

