---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VE3IT2N%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClLMdATMIKvdmiFtdVOWgSn1UiZVy4pPhopKLMd4KiawIhAJ6m%2F833Fc5EoY6UuCITpgLfeOmjLDTGp8rCQrWnAOc8Kv8DCG0QABoMNjM3NDIzMTgzODA1IgxsufzjMvsByut505Qq3AOT9qTKS%2Fidhe7uT5AAdFfEzRuml7v3ej9%2F6QRigkjIXrP9lFEENSGgNjC3SWYEX4%2F4wkE60c%2FEy551qOw%2BkVKljUiIL3%2BV0GkBWuPR3HicjcmupAJ0Yk3jzwfyEZxcR0L46osQywcie%2BbPlTty5ETieqCGYkeP%2BFoB0Q5bABzb%2BVaGXfTHypSRlCA25npgR4BswyC7tkQZ4Xnf64%2FG7a7SQy153zviJ09TjMpl6a6aFj5AG0zzSFlFEHFNXSN%2BrKY1gBOED2rQhrHpRzqheIYkhTx%2F%2BtNR7o4P29ecVvFtB4lICvQgL0wif2PjEFExKEF80%2F6QiKCWQxBZ5xTKkEYQMCTD9IBoLQ1GQvYz%2Fwe34WSrAtgJJLtfpxOcdFzXMcJfKiBEMmXXbqIoqU%2BirTwU4gbyfoujCnAvcZ%2F6Xix2tygSmXyYKJPfcEhTsAkNNU6zw7kuCN31%2B7Mxd5FNx5kI58kFqwxtCLc09iIy8hQSNp8fkw1x3fyf9Pa6IAJacUVgWGirOfzK6tsH6Lv8bSD%2BaU8HO%2FHbSHTlKTYbAHCNVSt2M%2BujZ7UQcf8XGYy7W4NBQvcHydTrppYLBq0wPbzDZFlJXDSjpda8WNCPnTeeKEE4qKdGLs36GgNC2DDahdPGBjqkARSuRhjajxnVSUDzBdmqLXKSOpdXQo9X6gBkgxSxywEuULnizLS8Wy5%2FsX4FmwyBKuLQP3Sw9v%2FKtLzyhB%2BWsAfoRNbfzlahgtaCWzfwEJ4eg9GOtkUmu5MJntrVSRaVsbKLPByIkRgkXk2tVg2AvrHSpwUrR1QvArwWflxOOWfspOLTRuRej0BAVWsBnXS%2FWTMedunwpnRa9EJj76M47DWSNrmz&X-Amz-Signature=222af9ea9f1bc96ce15394a470d08abee1b06ce834d95d0f9f6bc9bfb2627815&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

