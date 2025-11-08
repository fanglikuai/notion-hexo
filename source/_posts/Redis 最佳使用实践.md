---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5A2QUIG%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIDg7yIcD0EvZH8iRKUNgaJZgvo6RNnyzMKo7Ae%2FHYjXiAiB7CL2AFgUCq4QD7VzXG34XibnFdDpmQ4K2bln%2FJSNGVCqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUt%2F4GYfv2YSOP7zEKtwD%2FjojGXMkIS5b7ao9eD9rG%2BcXAZR0MmloO%2F%2Fxz3vr8Q7tmjSDYEocWpSzpJDaeLrzIaqpuVwGNPG43sR4lVgyynWBAs3qgSlhMmMJktgBKCCe5JcHOQepHdmvUMS5GZLgfYPD1w9OH9N1JoqrzXhD1rykZfd60Ibvz75PD839i8m3R%2BH2E%2Fqd2zOcvrINCpPl4FVfb1u%2FgUnIULiqlOTP3RZa8ZYKPxoqwTE6IbMZUSGPumdHjlv2wicj99UHfPzWZ63TouDJLuRDzH9Oa0PvaCJc03H2wELuLCsTpPM3eewZq1n61mrm4PWn4WyUpKO6wzFgHvT1wDDf%2BmXsOv%2B1YW8bT5nceVzKoP1EBa5phb1RYH5HRgq6i%2BpO5jCpsku7Vke4E31Ke0OsfD9McF7yxYbVMcCwGiPQMi%2FUZ1gsIqHSDS1g7ob9aVbeJV73GmqtnZHqs7v5%2B0cs5AFr0VYdNSmjapwXNwWldEpT3wUpJnpvnhA3skXIRS0EddUo7FWXRkNYJSqgEAtDWH%2F4qAeEx0HgmaPNyCOpqB6iONiSGokMiu%2Frg9aD2Xrp%2FDG5x8%2FuaRiHTNwZn%2FMP1oSr9%2BLNuvQRSY7Akw1Q0bzGLRmAEAOuCwtEpyBr3bucjmIw7%2B67yAY6pgGo%2BA%2BC2CntOCdxw5TdC9UWJu1pAKeN6w0tAf%2Fgx%2FsUGD7B57pms8R%2BlJFzGdATvJzj58siIKi%2BJWe%2FfL9%2FGdvqBJeboD9Q%2BpMWvaERqarFmU%2FXfDW2OJVp%2FTUIa4Xk8QW4ezghLptbYH%2Fh3rKP0tiBvKRwhZ%2BaZNVWGfXP8M0m48Wf1Fp9sStZAXZLjFU%2FNesx2107MFvFCQ3OZYfz0wTjJGDHROwE&X-Amz-Signature=a6968f439f5b355f9fea789c577cbd72e1e81aa81ea86860fd831197bea9bb34&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

