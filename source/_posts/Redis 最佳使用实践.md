---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665W7G5HKS%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIDMDqiTYHW6v7%2FaZlcjNgFGoI%2FgCXRpfTSocSbLb1HfxAiAT2ufECkkrwZKxCYEIisVjvdtQZ5Ea6tlkNy%2F%2FCatvACqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeAaaNnyqPzLk4YklKtwDEEZbzSDqme%2FJh40%2F31brK3m7AiTVOVaGJpgovKbzFmluqD%2F744nStMgrQ7epd%2BHKsPdlgazdfGUJlkVhb%2BLGO49jnDXWAhbjx3eCR4trrWvBZ%2FUswfYtHtgQKFxrcAB6zO3vrzhaI5KMgkPniGKNtVRlEVa3IYc7BRgE4yd73Is6ejF25NLQZHmiXmCur%2FRHR9af%2Fc8ZS6ZSSMV6dGeTQ0%2Fz2U12lJxKDSAeDyFcX59CGY5GTT6PGS9MkmYKyiNHq%2BI2a1ZoVSQW4E2DBKYWreIG04vYptqcHBB473KJsM%2BCiNxcNSNdL4X3ybTAULVRAjvHfrwqKiscV2wU40Sxf6lVZgInWxpmtqKvwbRGEaKNcQT1K4Oru4H1N0lCobvUPyqDGaEx6lIoGGhE64d9XpS%2FqnB6O5FRivnyGdvmmrDjPZ9R9UeDD1IMeZ3J%2BXSYOA82GLptcS%2FqO9mzEngiSTgYgKze8D0SZ9sYpUIUFEqvjTxFlSa4RNafpGuCLvgDawO6NnIblxKpJexxcx3vLKSWej%2B9RbeFyTKXMfuDAbSYUIbV3PrMzkXhkXrnyHSSYdwPaCLg9WYwIV7zlCjxZnk5a3oW2gpluUFHyaTWvWYJSVo9umiSprRVrQUw1%2BWGyAY6pgEMua1lgdjTHTQ7b1HgcmRiLO5OyiC%2BLuri888cw8VVk8PBqAinNr3Pq%2BCievtNo0nWN8danVRAUXxjyIaa0Eddq%2BQTLj7L8L7oSCRtdIaKCVsRVqaSgQ%2FIHTKFj5TJHlb%2B5y8dh%2FRnAtyv5Yx%2FtANu1YpohobSYQcmLxHLocG%2BfyKtpjDWoUgi6bcZmbD%2B5zrxOh8PMkdooXOxpZ6q93Z5TSQWNO4%2B&X-Amz-Signature=1508e6474a0748a2cd3e2504ad9609163e83dc64b735eb264b1d037519d6344d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

