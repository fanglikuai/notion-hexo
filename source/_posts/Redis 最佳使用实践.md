---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHFZTHNP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHDJWIP%2FHFoQbEOMk2pCCA%2F9MLG%2BG1aTe5bs13Lj9NNdAiBzkO7LRgkFBhxCaEe4sUHYyz27idt2EyYz5MvPqSz27ir%2FAwgyEAAaDDYzNzQyMzE4MzgwNSIM02oqGKtdhhhCSRjtKtwDR1n5faGop1k%2BjqnuIbhPbzGCHRvjtQ98m2yP6Rfelr3LYG%2BWi5BbVILhWj0CLRfC1pVICoFeThMBtjCN3aM3dA1%2FeJfp%2FtGfpIZz6BDLvnuJQsD2OOnE6ulPOGgFvN6fTTOXZc1cfzCRg3Fd8Tp2tSTqOEDln3v8K2OjwlxR0dQfY%2FOLwgIJneusuuAgleTafmWu1AIDylPuAr36HSgQvh0SJT8tekoFOrTRrXYWVbdDmt2V%2BXDwh6eOm5Fa%2Bw%2FlBR2thPzMZU4352inldlPNOTBzHqOuuz4dOdJexwAMbJC%2B73m9ClLEN8zXyVP%2B%2BNciAAxHb8xgOnw7pc4wiMoItQQuwEwBNTEwhpUrk4jlOopAQdUnz7rOf9HVB5XccgEcS8HHtf0w6SDxvIICCzzL0qrvzW2Rz6aUBZ4Yi59pBzHX2Tgv4Mdvj0bIHVhdgEtXFXosrI%2BhylrLNAuSs%2FkJB%2B5NddzFm%2BnE1iGB1ZDc5j50MlzlL%2FQJjzA7bx%2BdQ82GExRXPjzh5k4gHvUB9ieQs9bMnQaNYDTgieouS7BfCaFKKzNqylvW69VXJpRe%2Bu2iWt40gwT0ZMR474cCWjwSApahTbrGRuorkwLv%2BpCFW78nFC7M3sDCwAjs5gwxc76xgY6pgGVlCoQm6F0a%2FZleU2WsvTViOyEo6541nl4zqyckZe4%2BrdasTs07UnhFiPChy4hfNFVyeSNu1UgbUzSCV%2FAYiq%2FIHwLa6gEeA7sfSw2dcHt%2BG9FmO00ovFDi%2BmUMIVLGmrq2DzX0oxVoyxulbTGYyDZjeTxmA7eAUw367qa5TS6iLtO6sgDplbEnBddjQuJZyvi%2Fvjc6JQvRXUg5AFnJuEqUElpF42S&X-Amz-Signature=1fe7c5a250463ff2d212487a99e3355479927c2e195c22ce1d7ce95a1beeac71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

