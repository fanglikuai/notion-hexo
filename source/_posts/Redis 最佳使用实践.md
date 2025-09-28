---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDJASEAF%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIFYobWmFcLSjv%2B5m42HGoOjG1yeDhN%2FXMwXAUtmM7q7dAiBI19snlBthXMiux5GE6KntZ84MlLikeyc8hGKJar9VhSqIBAi7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQCAOpQrkiN91PtAvKtwDzSvVA%2Bd77yCjU0xPCeN03FMLxTh44Grll1AaClnMu41sAUWWOXkGt7oFVyTMPFaR6JDVJr%2BURQZdJ5h5t4wKGwmJqtwr1f4KBX627RONg8WBdu3f%2FtlLRMKQtaSMVRKaXYqGu36ddiWOg%2BRj1wu110zqVIgMBn8XDjXO6M50an5ScBvFid7L8YBU3ehBZN5nEoWk82Q7OC39ZW8QfctuEe8q66Uewi2dC1MWRgjVTyZm7E9DCS9f6%2FA0g6j1wxXxSlr%2Ft78OlRBVWXeqW502GlmbuZWYIQGig45iIg4oS0dfy%2Bddd1IlyGQhjlbq7inh3YaEwiK9FmV7Vr6eT93A6MLg3TaHMSMrJV7cC3EXr1uoH6EY%2FkpwcCnLUnvOF3JYUaEBZx64lQlaoc%2BO%2BsX3Odzh0qSOY6gl2716iLwG4O8xhrgWDvlxGZCbGFAV8o%2BBzL0rztNqtWPJt4fmjCpC%2FNs3a9m4gV38OcQ3ad005vPADm2V2p0Vk4Q4A5Ev8IXLXkNRVztgqArL79VwNzqkldUFm6C4GqsgsjnrgTxfWcLoRsNegkcWEjI3wGls8Dp4AThDI5Alj9yG1eBXTODYzI11y8RcZzi3JddPsPxSrwX3roRAKHxV9b2UDCMww47kxgY6pgEjAi3kniMHYEHVc4%2BnLz28isSux9pWH8LwHJlVjnjPjlwwsXRNPBe32rbsSezPub75TyprtN3fBIrCGnO%2FyXY26ag2nYfHpQ4Ti9gM%2BfXhFFmMkVRLF2kXDxzsrlgieCFtW9cb7%2BOZGZvWIouFcPDWH7QNbYp34NN19Cl%2FlkfU4tDIn5Dzt1oUEHSQpT4gTghHWcVd5fuRo3%2FU2goTVh5lwDDLHLx1&X-Amz-Signature=872302329865b65e9519bb7cc15cf0630019e987fe4730ea95511ee7698b4fe3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

