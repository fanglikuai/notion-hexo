---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667VRJL2FY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDuw%2Fb4BrWq85t1jP2CX2L%2FKhdOcgM7Bf7BTJeLhLoQBQIhAO4ZXwB9YQuA%2FL9rxfCMgx88MhYPPGDlaNmd7WopNO%2BAKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxSPVGicUItyeWuPT8q3AN0Cowtw8HGxt7yRa%2BAX%2FFWzmSM%2FVWt1%2F9KkWjgq7r3LC5g2CyOxh2qvpevPyDtO8isbGtkm8mrGQEAFQwbooUGSe4nplHTZwD8IE9TokngIV4TrRVbe0Uy6R2i2t3RWez0hUCg3h0bAEUUv6UxqeT9f2KBadOL9RGwyJlEp6dt4IQOLtNdqRTzX8AY27fp%2BOp0vyYzCfGkuHULahXW1uFBd%2FR8KiT0o5o25Mo1tIMQ7QsmXyve5gdPx6GiYxIou1DwzxEUqkSPD7Iayz2OU%2Bt5ub1lX5fFcqMz8GMf23emtHVeZxgOIol3vV1eBfm79pbsd%2FA4%2FzAcKgLo88TCaLHRt10NAahIM1ZVWFXlt8DAw7j2vc8cXrko9BX0MPqavcw3oGixB3uir8Q3oXtdCpXG1rDk1R43DLY6Qlo%2BLotHQNF4OGBKKcEnSLR7C0zldQF0MCW%2BdZEmgs3%2B0I8iVSlhHpUlgleWHHO1r8ul0ZGZO6GstdpZLzGLw%2Ba1UQjg%2F7OxvkMkO4ubmXIMcv0%2BAEnyTxtqGRf%2Bw8AktVwtnbT0xo5oa99%2BsSdwsrT6j56eM1iFcLbRbdyahF0ZIepF9GHFNkmNehspBKhpOD6OHkcxbAfHD238fH9eP6snvjDJ7r%2FIBjqkARkydrzxazCcyVIL3duf5SjP5uxYT8kmP6%2F%2FEUTGXCK2%2BS0ZgFINLqx2%2FcKVMmYC%2FDMObupEOFqky4o6FitjKxInT254B7xOenpjEmDHsE7Db99DQJBhJVw16kTL7K%2FNm88AyWID0zC6yIcVAtw4E22vSjwFuXltFiwoSpHkuX6vkLGDbRZAptPqIPDKw40eBGVTXuI0S5KrIU4ic6l0dtHSZcpc&X-Amz-Signature=f7e2e7dffc2d7f7d103cc609ee9ac699a30965e6023556f588b045319118b5b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

