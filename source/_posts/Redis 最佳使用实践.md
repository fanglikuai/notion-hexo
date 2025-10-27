---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJ7N2MGI%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCrk2BgMyesf9v5U4b7vb26b7%2FpCGS9WJdlO%2B5IuMuXQgIgGksOMbkInpt4sCy7nMYrd63GvvUIHJbWmBYWcmdlgJIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5DKEh67xzkxo%2BdESrcA3LQAChjL1Gc63wZxZyc5HOdyTun0Yt8NGeKQgiAqn%2BEEMgB5kgSnhpiZpcPuybFWdPKtmnLiGPAVUZNcdL3aAhcoEB64O2ApE9w%2FQfuacKX1xPLhmWX0t4S4OyB7e1A0CWq8uk1lqvi1yJi2LNz0iFxCT0tp%2FPcQTZgNTCKwEveX8TGVTD3B33KO1XERjkR%2F8pmwT6iaIlLIYGsat0x1yPMFarCHbd5XZVnCATMNpBY%2BeX5DmT38Arvzjx%2B185oBzInZGSqLIny94xkyNOz2aXPc%2B0%2BRgoG6wp2OhBEfE4mYzcpxQJHrK2TEncbJGt1bn%2BKvWegR33y4r1FGf4oiDhDh9QOwZHbr19WhkhCKjh9dT2fjJQojODdhDgcRWWVJRJ4qL5f7GAn9IL5pOcon%2FglKzybWkFi6eH%2BuGn0WjeAXa597cy0ULprvWKMBNg%2FeRsIdhMktFKurvWQ3MsuLMyPbn5w38%2BYIDZ8u2TyY4Whhh9jjRtYWTi6LwO32DLL7RvLBa6B67Czb3vkYwM496fMTFfHrYYiBDx%2FRh87oMa3en%2FBPp019dJvQk1LUQmf1kq0ZACEst0QlxGW4QGEHugRVEvwAPXLPvSbcpE8BklBr28tR83IP9pvWEFWMJ3y%2B8cGOqUBzDpXkp1Al5shKEsUgwlj1z67j8e%2B1lA6uxeDioeT97PfA4GbO6QH4dqmSoq5O6LaCVm2spiU%2BmXdKhN8sf8c2QbwGQrmbggbocMQLbJO2J7Wp8vF12nLNgUjTdEVeZPLdX%2F4VwLcr7%2B0EzyNRRBTlg5C2y2be%2F3FTSRFa%2FmYNQYBkwmFQwf9xh%2BCfbyncOikzXzAoKxRKLM76eF8dJiBajKeSQjQ&X-Amz-Signature=8f77c1306aba48b4432ba594d2d15b141ee88bc756bf98452e0c0728228d733d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

