---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKH56EXX%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIAiSxzsK%2FGhkO3oha3aI0giaptCG0AcexDXiCPS8LY8XAiEA%2B6nx4MGGugSsV%2BP1YRkQhARTE9MCwL2eoDjG62PhNr4qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDItIzoIT1wP1yNJzFyrcAywu1%2FfjFyEsk8EpwnBJe19FUZQEVoaxsJXOFoy5Yn2ONBw0GgR1wh909kqf5WJH40YUwISGsttioBhOql2nTtevHMXY0xafhRCrWM28HCGHwA0MRT1sok2%2F2UgvpQi0%2BnNfx8WJ%2Fm4suo7xngQIKjkjn5ZbHqIlk%2FxHNp3vttVwc3g42%2BAaOWf2cFxtJTHWXQPP0xbOj3voEPm3cOnmADTULu71bxy2aVQWqK5%2FaI91R%2B7Ct4PLmz2R1wngTjIRKMyo4TEjnopNaaaz4A0X8hzQOq6PrSQP5nvKP5dxX%2B%2FO4pozTKJlxPymfA7seG9ONXUJTFyiMtRYHsLDcarcVgy%2BTQxQD5KAlnMzg112DYcik%2FRAAQINM3DRpI1L77P4rVEJjhYdTWAlk2O3YSHjE4kvOET%2BzCaVxreDG1jOPEDNFeWr1D2AB4TxEBwkXrxHaw4Dl4u8on44AlPhGhRovQs4YAjHTsHxtI%2BZjJglOroLVuvg6%2FMgQEXMHhB55T0FTLn7C7QfV%2B5nLN9Cpuob8GVRSi2gs3TXJsKvxze8mJELHFHcCzo8cLduIVFHhNwsjnaZfx6Akm50sqHMoMjc%2F5dFpMdUEJsayYt5TaFjrHw4Gh8mRS7rNH0bWqQCMOO5rsYGOqUB2CsKjyn9E9orfBbseViAcyxbzkJqgs7qglw1eDpRu1Tv06nc2dtWvYcBkF4tDuv71yoYBdFbgpkscgZwHZ%2FpOk3DPZzPNfKq%2FUDWWQrfszuv3U48yQIUpMrtW%2BzoxkfCWOiRWHFPRq9m5fWo%2BVftNeIvQa0NUv3h2cvpFRbcCavx9MsApvvwBsjf6OF8mY5GRYehEYgk1o1p8B2Ec85u6HS9jyXS&X-Amz-Signature=938a2b82111cc703b3318127bd3883903d84d994255d04641ddbb8c5a0d5e7a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

