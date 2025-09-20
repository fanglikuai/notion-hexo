---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMCWMGXF%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIFszqLMW0PLdIe8xolDZ8%2BUSCISfCV8uYx3KBzqyiTSgAiBUXAfv8JWGFJXCYRBq%2FDj7CviyTpmhsAlKmJ0toToL7yqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIQtraVSavlYORWKvKtwDXac1J0SlRH67jIbiFpA2AXsGiv8CyzJvoscopkedHLjA%2Fl9uqtGoei0lFhPhuIYRVNEFlxA6nFGF1I7HY0ZprNKqrnp8%2FqHZUntIEtKOfDFXwVXCTuFpfoGxM2TlxtdHbLor2CG%2FMfTzmNKoyYfw35d0mH0bEVNiRIHZIKZTG%2Be6II5JBIcoi7PalT%2BlgAzDZbbMyzRL7AvEQD6bVQMnU8tEHlczdv91O%2B0ipBVP%2FVS4vOx1bOzIzvQg2Fd4YbYuW%2FVdyetH%2B1U3fcHOVbNwRjxvA2xYvxPbq6vtbZridC%2FtcKG7ZMHCIEP2BcPnzsQP40LD3RF%2BOGCrWyAekTvUphgywPap9mkGzNvsctCHOOu20cWxRzxa6bUy%2ByvN%2BaGicBhq5o%2FScMEzuePwZadmAHFrhpPLRDkLlhAL5syuAUPc1xILF4jCRCWdc173txGCniMkiZtvACybxd%2FXB1ozum2VcuU%2B2OEEevXYnjWG%2Fdlds2za3X%2BN%2BpaoY%2FRygxgmd7Q0I7qBD5Y%2Bg9I9y3T6qf2UsfIomdQjl9pmB%2B%2BhvNIijzMZfRTRqWsZxaRmXE0R6HkU4LxUc4AtKA5FIUzaEd3bRDbvXHEw9vznTxIL7259%2BGWiqxM8nLX5amMw08u6xgY6pgHTQEzfwIpsfG1RSd0XgmOCNchvMv3EyfzvvptXUV5AsX5OQ3GToDSTxwyXLBORZKlmaGT6Ebz6sNIruSrGfGahwWCF94OkgeCzOL4ZCS8B3uh0n%2Buj3dpECZMO14Ht2zVRfd62%2BGqGMXFgxLMGkM68NV%2BbeWtRIk4soyp5S1lOcRwBnnMgORk8zA1KBTkzPbqrXKVAe1hUFokzt7lUez5OHdJE%2F0Wp&X-Amz-Signature=d1e1d13f5b5de42c6289735d1ab7f8556a6b53e28f2cc3d90a23e079f9066976&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

