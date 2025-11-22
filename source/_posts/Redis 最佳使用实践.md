---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4FGLX63%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIDyQ%2BOJbYWAQoOu5a9mrSu9111cGefZDiuV4p3t4AHDgAiA7G8k6pbhFcfvjDgBdM8Gcztwhc9cGSZ33jZnDvjFEnSr%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMCeYvbbSHd%2FbixWfUKtwDb9EwimqbuoBc2vS1WcCBQDfCD7kWgx%2By3xZThnBjsmT59a2YU0v9yo1NYik0VG6C9sz8c%2BwTtlLXWcpyJv91cCtK31iZbjXracUJUnbLYg57UeLRqA%2BxbSXBigTO7QZhG8wcdq2CQ2ca889U1AECreyYW9fYq5Dl5uhzxP6JKtLuxgv5yzNvmbeipabDMnQEeMUuukQMJ4%2BKtlXaXy5%2FKGp0OQWBtV7dRbtfsMCIFFpPS8yF1aXETWH8mFYDPuNbMSRgOdTO%2F6R%2FLz4QaF7hZJ3vbezaXXFgjYTAfGqZI1wu6X2BnQ0TXBruotQ3pvBXbHNsoE7VwUqNGQW9WX0ebq%2FvktJyS9N8TgX%2FaA414fC35aJ96xrjFLFhQdMK9nsmeCrTA5UtbVSvqUm4rBhyCkMYGcg%2FbV3jiBdoxbjSLzPLWKzjLdCxwRETBGuMqYVqnqvgF2PQiWlzdNhaTA4kYSn8PJn67OexrFH1zr8No%2FwjvBSi0P43D1je7Yu0NWopYQn9FORlSxSjwfGJZ8C4TU56W4rkbdxEq5ELYx2lm0JsJMqi3%2FdFuzvdGhSphQZJOgxDOUFv75JTDELcZvw2lCK3YhEEmJuC0ytJX%2FphXLM4TC48m%2BxI0q6eGzUwzYCIyQY6pgEPUhjLtK1A2e%2BCj40HWelIbf1xz8l4fjkjBnBHeeKLenJLOlpAxLd0VxuZ3AYkEVvEmGnF3WFhr0JQ1f9ZGMLxuBBr8FRjGswSLS%2F72XJHNhLCCXFX0gKvqMbp3dw5obJREsT2KrNy7vppiyOU2IJMolRP48GUgQtkozUAVXXuP1bqTsyz%2Fk2AUOpCPEF7k9KYI%2BD0k8MFlHGIjLk46EVE%2BWzxe1cu&X-Amz-Signature=e2f6b8b0f431087f77614c010f5b41a31ae0817e607a862e5775817a44f2e395&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

