---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FZDBIPZ%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJIMEYCIQClnkBsIAROV1CR9JyjHhdLBkeGZPWVNBORP1tA2o9foQIhAPrJbX3uNpP7MDrIlWozcmeDdGKHCYeLg5fVW8LqzBsuKv8DCD4QABoMNjM3NDIzMTgzODA1IgwkjuNbBhnVwwWOf%2F8q3APNlkgaSeiLdJ%2FxXWoV4VOVqp5sLy32SgRevm36XLjHr6%2BWmU3WfB63LErypGxgDPrxWQXHg3FUdfdjSY9240Fdq7%2FKLd%2BB0n%2Bpe%2BwK3FB%2FXETv%2FKpLvrhF4YGm3pMF9LylJwddsOh0iTEV0mEu7E9KHojLQ2lT5BzcfUfHvuwcIBdpNgY3VwRqfq3xaUCRsMqpsHkVPUh7pOS07Uru3LIWSNIktSscb%2BJLG%2Fp1QEwjBhqBIP%2FRcELiubX%2Fx5Tsl3P7MLXWsUfbfdAaf3HvM3UL6ebz30sFQSRLAbLwxWlW0%2FzFMddWB0%2F%2FIj706O%2B%2F6trcQ%2FjaFUjzFJuzgCWHe%2BTLM1XOZZezxIvyAQayueWaLCVKQWelKgzMDTBmRz5wQ%2FbU6htKBWtB4arsJ2Ar2IP4ctcLhR%2FA7oAf%2FMZm44eP0UjhCGmQMlCd1dPDmZh4tY0rKXSRSTJwjPb2OqUgtbSUwl3Pol2OziT9Yg4tlzlZX2skyrED2LKADwRMe80KMLYSNMZNeXgxyUmS3dYmpPxbLaoRTlWSC2H8ce9n27f8Aj3s9w1b9N%2FGt5F1b5Wjv6wUmRu38UYvY44wfW6mOCb1L0gp1wVycUaax0JHjBa5LOd6eymvCGrvzkKT%2FDDf05vIBjqkAXjK%2BjAOBaMxYLNmZmmFpfcZ3qbMO7RTmwyDKJhZv7aClfcKnLC1i5bDQvNquG29u3UrtwJEdjMxuDdChPjtL2cxX1KV6oCIN6HrDQpJbynQq4xis3BzCU%2FZVR2z2VTMfL%2F6ykbZrxP1x1u5LXPTr6Yd1K656QsuUbKpYRP8xnTJhZ7oqc5NmVFQEIH37PQLzHPpMuH9KgGCt9GUW53FSPfoUnBs&X-Amz-Signature=efd1f056bf75d8a381eef25221869dacbfb20ba213b49e5033c0936a763bcf70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

