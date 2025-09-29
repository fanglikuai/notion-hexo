---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ6OOA64%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJIMEYCIQDd1sGhjf%2FUlujCUEDjOyyqZfFOmHaguxGTKeAyKhawOwIhAMb%2FydYA%2Fs%2BJSaTfo4hArtea5hWnTiOHFWyCpZvLxPpPKogECNT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxA5DL7ytMg8UL%2FQbIq3ANmSpeRAVQ%2BrsJj3FYcWCiGdY3ce73dbk2itohfLLY7bUE9du%2F%2B6lgdq%2Bls7qCUYCCvaGsQikSOTB8Y11Gs7%2B67ZcqIds6o2ODDEAcuBXr6%2FJYnDtBqh0al6Rbfhy4JH%2BpEMkbYRDXOMDOACR1AgPoAYWfsZyxzdDLJd%2BWwGNGVNEq%2BpYPG5QV6J99b2m%2F1U3GBBjHogWCKjYnawRWY17dlRQM1QpZTAIiiCuuD6kCDj%2BzZO%2Fkjrw7nAnA71avC5jADitzy073aBA49WTRmJjh5pH0Odw44lixISAD3HfVTYEXDY%2F2CwwQno8DsnmVw7y%2FAl7LGqJ55x6QmM5q%2BQfpRX0VGWO%2B08ZHxs79UlU4T1JxfW2GWQk1Opa8G0BY0dRUP9BZcrd38pfRxpQi8wdy7zbScExhxEZdA%2FasVtEl%2F2QLamc3L1CVH%2B3sZvzaXhi3magH6BwVFgolQk9Wr9DUzyXRdgMt4LzAwD3t4VPORhpOgaEDvPHq9IWXCddNaqLjgBlg3Jp3E4gPVD4bu03n8u6DrIg80B8II5j2ttm12%2BCW5JWftIyMQNmBlxm2%2BNgry8lEOgwz1Lii0xgHMmF4exy9m%2F7cRsxoPMVD1w0daBLd53LhtgOlIq1OGdzCz1unGBjqkAQbab4%2Fuw1lZXfcc7BeciT63zX69F%2BtlToF6H2OyiXewtLBB%2FgDfqP0sbskmTC7of%2FxWiniberpdMuAPWyQxYFZ2SBXXF%2FjRu3cUMcdeuWdH%2FXV5EWyrS%2Fyu1aGA%2FUbjURW%2BxzIkkCmRyl1DFfhFh62fPLbT3ppkS%2BWylcH3bq96brl5OrIGSNS7UDXGvR4wHLc0Eknpbs2f944JUMdSczet%2FOD5&X-Amz-Signature=2105513baa6733c6cd390d7b2120da46dfc9d23ff80b7bd7377e938611b4a750&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

