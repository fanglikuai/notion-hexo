---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QR22CFBI%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T150140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQDvcs2APVE47m%2FJpzivmLFwT264MKVOndWEw3R4BwpacQIhAM4BsB3J571dUp2pMmci8Rhyy9fJhaHFxcwK45aa46bwKv8DCBcQABoMNjM3NDIzMTgzODA1IgzStj0ZgeoGqbuQeC4q3AO3gJNLFmjqtkduI%2Fi7SQEnYa%2Faeid91m8AG2KVsKjCdghXif7y%2BhstAmdepNPM9FIqQaZ4DmCfCeoG1abvWBuAznxWr1jsmXhVyKuXs4bz8GZCta5cCiQL1dsab0Iy4Y6Uft%2BrVQwWpqW3Adpb7jVucIng5kye0IF%2Fj33%2FwNz8xvTIEgERb59W8L9fRjLqvOA%2BNFtFcdL7rXYZwyWoe6b8ha89qX0XkSIXDlwxw4E9SH9tDGddK%2BNttLQwHKcxPeZ68R6%2BRWbBTmvu49X3ViN469E7%2Bnd9R9NCxpdU5LYlY6Qm4Eqd9Qc42OCGeuvKrt5pKDGTCaQ9DhZIYLdK0YkKSyl%2F5DjmIrO9aDohMBVA0l3WNJTYpQLzOY4PT%2BkjhoUOCJ7jzsq6WAOFIiBgKAqD3MznIYtmBBdXfXMpr3h%2BjikKtxLXPCqMQPH0DFWnYJDsCd%2BeCxjPXwGbst1Lcjrjf3psMnwkZokqaQ9xr%2BNALSClsqj0jUbeABOJvdQt9Gdoz4HRmRE%2BPMCmrh3y%2FU2rmAErG6deXUMKSW4OUNrwbh9Xyir7UAQKCdzx5mGMGdJvB6DtE6sfSk14IC5uf5efYzm%2B7i%2F2Uz6MpefBr0wOMEsBDYOy%2FHShMZkzhTCprt7HBjqkASL8KRW1l9%2FdLPj8C8MgEokV2JVGwJ%2FHDPsoXqK3PMJAfQUhgwfbKz1tnQEoRBh1HI9hY0HhzFLLj90ZiWHNswvAGx0z4Pm8Ez3rC5UZrtKvZ1GB6HUHIN5EFS3Bdd13q%2Bqp9JNNRXcbk2dPuF7roP3E3XjETLeT1GaIl8%2FPKiUhYbjCGnZ5EHI%2BR%2Fp%2Bq0AonGJNUKb6UyFmmp6jKLC%2BDJBRJG2P&X-Amz-Signature=4f576a8755f12f35719ff7eb1a4dc217018e8e3eaea678dd95b718de72c87f72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

