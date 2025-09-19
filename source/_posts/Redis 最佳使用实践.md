---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672Y5W36U%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJGMEQCIDvimbkKbfjwyrBPQ2U%2Bfi1FAX0cEq%2FY1Usj4Wd0TCMOAiAQq86VmGf6OvOH6DiFoczaQalP4bpm6GpcNRWwn8VTOCqIBAjX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BNTIoitjIucLb2P6KtwDFceBUktuyVs7lA2SnGWN%2FG2bPvovXj8vQFjv%2F8YPSnPcAacMRj1FUTNMrpZic7t3CaJIg3HCqCLsAmp%2BcaFTMV0ICGJtnah9jSZQQWcJlSMn1TMpqxNPd4S%2BcYqb810%2BOc2cIxgHzQ3fi1%2BRSIFri%2F25Gv4lpYLykAqmBjNWw28uW0kiIZA5EpZbDSRZDa70lsi0FFjda%2FCyDyaOxFs24Y%2BND9u73TAZaFTjPnhnmo%2Fvfam7q0awzh%2Brqig68Lq1HAOVFDIvseWG0xWRM80btbdxCiC879AyxCQNzHfVXFA11%2Fm69bpmIjip%2FS9xs13tR4XrCRHMngI87ykuo%2FrntgsH7bRxPPfCoufOMqYB8mza0mXpYJ80rYIDVubUrwjDEpUiDPoUxmk%2FKluwmcyZMGxS1WQt3LF38p%2FnS6Z6QlA250gIgdnAmTuTdBBpDU4h9Di%2FdMi2CknoujLzQ3bp%2B7gxSL0bUbRsKuC26TCmtyfsS5u6UyeZ%2BK6CFH4Ux3yatp6gqPmrJiR%2FrOIx%2FJ9ErRfsfUnQ3AQEdV0qQOZ%2FSR5OJ4Jyf2BUqmHf9g07q4DS1ZFmz1%2BmN%2FOv3Lr5kK1lkrCkIVxvNIDO1zWVoC7FJUHSF%2FZu5hbtNL9vJd8wmsa1xgY6pgGPYjkTi4TR7VwH8NN1%2BOWFogQaL5%2FcVHbU4xHFzvyjUZszSrV4mrrkqJsb%2FJk8lAE0%2Fc%2FQZNAsKljIXEeX7awHLgDpcoB2z6pkeKHECexzpAOiLIs%2FD%2BWKJq9jv2n8X78%2FmuOqc4nhRpLkxD9zMe17NFOyk6%2F2r3HRfupN%2FGFMe5xKO2dqkigef6qV4Nbb5QiyYPv6pdZuidOeb9PudDiRWepQGS%2FQ&X-Amz-Signature=4eecc1e13ce5b7e9734de8f68d3ca66cc9ea01006226b2ab2cb56cae789b8045&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

