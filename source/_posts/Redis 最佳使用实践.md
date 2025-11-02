---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AIK2T6N%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQCBpqazD5J5Hh9y2ftccOcjcegUHXxoLOOyQMCZz4Xw6gIgLJwHkFtxuntXkFbStrW6d%2FShESZZLtV9atINjAfjkmIq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDIPbMEb9E9UKG80Y0CrcA%2BqWAnZKCF5vCiC%2FisJnEueMN8Yxe6YvEiSj1Z6sDVmtbxArsPjwvvjT%2B7qJX2OOzb%2FklwozDtgn0qr9lf8ONlXONYjd%2FWt6iRn5GEv8VQS%2BGKfwCf4kkYnY3YtzuzzJjUSmboCyy1xFt%2FiZ88lc2WwY8Nfz1Ywaq4Y8oiNaY%2BZcKV5QXuB2fgidYVZs4dpQ1scjietk8PsVYLqCl99m4JLp7ncI629j1sO4DPDmpLLI7iyX2n854rBCcEzYzvbOfVOgTx7iy%2B0IBfSvqivYBmPHzWKlUl%2F%2F2BKq8Lrt0Pr%2BZHhUwgxv92lPmyIJuJIduU2TFMyvXPgj6XVf22ltVY5y%2F0c%2F6KBmogRAaXnerhtyF7Am3egA2fDAgY6zhpp8YiNZbmiVajtxc5s%2BDzsYJdjk6zlmCM9EA4r1S%2BZ%2BAuSBVnAZRMn9WJ1dNVr6HsLe0UwirrX%2FKEl43vsaJz5r0RAJjND0kBCGp8ZlHYi%2F2mZE5Bl8o5JnCgysZcMmjnLvD60IcLVxEhqAkIz5qhuSwkscggzpxJIq1zifWI5xtvEamcO8VYU1LSvBpvDM2%2F3ZNNAQnREY5boN6g%2B70MAaqGAWyQMvbuHoiRzZlASpzDKDy%2BjQaZBDFzBqvJ%2BGMOuUm8gGOqUB82JCyCRLYxyTA3D1Mfszl86qfwblr5itQDg4R3OuDjpiqh4g%2BFLSutAbjofkqdxYwBpCVBU2Oc0OxWFhQvun6aplCc63aDJfBy4tbyYnrO7wudEXFmoi2AmzYDjsQKbVBs%2Fs126kG45D%2FVphWU8PWTKyEvVvtZs1gE4bfJeP%2Bqh7fdaweuk8FQJmlBoVJnSTna0x%2Bvd5xTkBJxiAy5%2FQFt5%2FShMr&X-Amz-Signature=6ebb778cd83a7e33fd6ef2f375ae6aaa2c760c33a1ba821564347e719d15133a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

