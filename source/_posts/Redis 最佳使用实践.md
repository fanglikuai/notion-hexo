---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667M2SVHDE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDt8%2B%2FclGcVdX1Njs1ND83bdQRNIERLyztTQUk6rdfsRgIgfx61T33tB8dMyooD3EBz1%2BdExD%2ByecQS1npj8upjb7sqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEQuzUrkd%2FhTpyvMvyrcAyl4FS7lDvd7JoBCUKJgFiAvdbId%2BqQmTQnDoRgvoivjCMi9lD8YSzE5WPM5doLah2um2QQqtAZ%2FgdIPrLzgW3WPi16W7SDTnR6d2Sjoydq5H0qJUQd0lfHkXdvQ%2F0U%2F6xm2Xh3Kxs7%2FPbZLSMgL3CTF1rQq5s39SIbSjex86erlJAW7Yw3K1OaBBOHf8zY3u8J3gc14u2%2BcbP%2FI%2BNY2JGg%2FLWdjQLPXAbHT6w1rcDz8NNEjB%2FB155MtmSdjCv%2FBXqS0XA0sdKtMIgTWIWF%2F4dAz%2FxnooU3b05H6kGUqLo55BzqxXZZ69QlOPq9vxpeLOwkefV5z6SccUCDbhELkNcUFFNIrKenm5AvMCLyFdkIhYKhwtb%2FX9LtVbm0qpaXb%2BtwS5o5h26tTTQcusxAUS3A0lEtnWd%2F%2B3zg7YAretlQ77q5u5qiGmdmFrqLYR02u%2FF743Hs5eDwEd8kH9CPElZVsPoOKlPQDxkPLbA74jKpbBUiKQITZ%2B7lwJIPqN9vGuMgxdQEhmNWBAQPl2soO2y67Z%2FTd6Ed7vER0y%2Fabgp5dybpJDZG%2BAweKfEijcRtCBR1RlJy%2F6J5V2vCTyv8NNsTJBsJCfVsMbHmysdkqk5QJdPmFvyn%2Fs0%2FwXio%2BMIv168gGOqUBy%2BrNiB6TA88WIZnsgKFb7swXX81tf7G%2FSlQfxdbZXhpbAanSXJuzDO85X%2Fy0RuL9fyzo3OuY70lCXRGwOZaRgdZBPCIOoq6PbchcsX9aKO2RHGDxCT4PAzMNLRcH8G9A0f%2Fa0vckVBne%2BYXS2wjHPeW58Xx9UwLisRSfDX5B18xniJwD3Lk7r2nCkLJVFKmvbMudIC%2BWuXAk5sscCVUVwauSdckS&X-Amz-Signature=339752ccf302027bb428572520e6b1ce78acfb6636abf8c69e32c6f48698746a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

