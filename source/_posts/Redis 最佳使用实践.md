---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKUPMCSU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEz5ie2aGtOPXOlouC6GWYVNFv0pILcWlPNbhqgV7p8GAiEA0OUja%2Fz7xG7fwz%2FM8zUS35V2SYLUJL3jR18MMfzC3Ycq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDEvMLkbItH8dQLlVgyrcA%2Bil6M%2FzA1jV%2FFRpd35GqtAhsY%2Ba3%2FmeUgwQ4AxXNupdJKaAfg3a2a6hLRVrjooHceq2toRJdLpDM9DzwKGdqoYUyQgM2UXibahchM3gb6ERKLqU1vCFFRm7qiHQ6NWodZKbQ%2B2Rn3PjaoQWuxtIgsj0bFgbld1nJgSFysgyPVXd0ZI%2B2NJkTbcvhmotLOC3LfQrCUm20Ru2S%2BP9fEiLumxmmAxO1kqvUGCiBlF8%2FTo0QmEyh1MlVwpj8IV8zGDZ7IapvAHpZKbQmbIUNRIo4yDnh6NC8BoC9JxCSJ%2BIs%2FCJK0L0eLW0bS76TDmDVXXSUnPMvmHTNzP7h6wb3PRFj5lwnHs344GRToORHVM%2BxEpodw%2BU5O5AmiO%2BdkIrF3pPtNt%2B4t9eH2nM%2B%2FKlQStx1gZ18AnVNPaZDXimEI8HNSEqSUedcHqaRizeeuT8cBhX69Zhcz30PWWaOReoAiivRC3vK%2B7ig%2FWAmTgiiFHVPLDkM%2BodCZ3iZfjK7HLFNjyN1B4ln0gHQsHJZ9xxvgOOHSlK30%2FiL0n1D3cwEc0J7GW20MYxpeM0kNLifaNEL%2FqPrzVLi%2BQa4g3%2FxDdabEfKPEpfbJRsUjianLnl%2BwBHtsj81ai4j1vDE38izIV2MMOKsMcGOqUBch9ykB0OEPBwbpQS3h8rzD4a0YFQNGnCWSIFX7strqh8E%2FMq0tkpZTue30ARmnqgwoJhh0Q4uK%2BpFQGLyJsJeTa5%2Fu0VJu6cEaJIZJnj0nyGRcqas2Xo0PoMxDMKLsKeJli%2FpCcCTLghjcAVP4z3Z8dLky3lIxpnRlIDWTLLvXFzeEJfvIIuAQD3a3I02EZllAWpgT%2BDN%2FtpY911GyBFhg0OxLf5&X-Amz-Signature=f01324ea7ac36dc6599435340d6145ce3896a889d77bf5414414bb09d81595e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

