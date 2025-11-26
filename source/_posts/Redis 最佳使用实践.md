---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CGFNKIE%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T190036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDq6dW0gDRvhtrfu416TJ4Nk56wKJUg0kNtyv90lISssAiA9Q07CYZFd7aj0cGV14mH4RXUBlHzASOG8fQ4uoDJIKSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMksGr2J3i08ljwK%2BEKtwDap6sxbvyl0Dinj57NrCvO6c8dzyav5U5Iz3Mfdc4BKiTFyWn6SVs%2BC4FPJqPAE5C4ayLzwVrb5SJJXcas052kuCeEnFZ2kAj2drI8VFWXBUADL3j0KOgFMIavxTz%2BWU4mcdIVt0vji%2FC0QP6rSoMZj1YgNTXbHGr4u8OgwlBwKnSk2j6wEiovPD0uw0gpLnKr24nBPOn8nJFffb1LbsLwniPdRmET6Jgz%2BiJRRJyaXEQ5YURon0z2qB5ufrJYqmYL7JPq4uq6oiSe751lKxHX9h9XLHON0NqAR9%2FYVt7Ok9hi4yqWx26DoM1khibnVdDiTxUVupUDpzt%2BqQ3RVIDjFVo3GtAcw9%2FWEbSKqRmUrFYQ%2Fz%2Bu1hLDXegiXwtoFgj2DBmPmvjAbypfYyCGvqzh2GwWAYBdUoTRzpKAeKNjFZKLRSovrmvAmgsO39NU1Bfy9lGAFA1bN7LKMG0b4nfaJh9dcrC2cnF2iQwVxnffvOcPDpdemwpuGDLRnSV3gumxCa34iVZpE0kYEB4xV1fEMoUq4jG%2FSXpIEVBHqlO5yZmwU9M2SZlCiYLjODCVZl31QwNFnuSi8zN6c7mHhlLmuz2oeQCYMR5WcXZUKHdPuyT4kUi%2BBeQpDEPiVcwpIidyQY6pgHk7dgr5yO0pDRjGLj18PjQkXS8Gi5B6qlPzb2d2z57yFDc32AueJZ7KIM6N5086O4AW4VKdzjORtMW1lyDQnUCxf9VhQlEi04uX%2FxmgarZoSEkpeB7GqxBKf51RW0u0eNUMwqKZ2DN9WlOeqTswnqoF%2Fq0mKslCVEwikNtWGVDvwJCPZMjuJ%2B4hvyp9SppDhNiz%2BmcI98794XAsfHksU4oy2QFCNZZ&X-Amz-Signature=0fceacc742dff2f2618b5016ca748174f3ee8c8d6eb1d4822699cb8dc3dfcfcf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

