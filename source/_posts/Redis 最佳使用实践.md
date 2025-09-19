---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SV2TJGXB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCePKFNWJcCH7eTXLSYnkvLuLlJomyJtbVUEDCw7NX%2BhwIgdi8PdDsJBRsDVUrH3fBHsarkpZuptPZz3EYZ1F3AKlsqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBcqcEUAze6JLkdo%2ByrcA%2BqU4bJOcSfHfxosOLfevMmfN9Z542CliQoqFDtImCWwC8OTDHiLKnIvMY6yn808xpZJFCs7jD2LuiBAN4FDO1fyrDq6uDcyuZNBhWEgwdM0mqMh8wLFbeWyYxeKHUNhbIPFdz6fj99MoGfIlKBkkXimmzflK6eP1ka0n7iJfNIrSz2qImS0hHx%2B4W8YIK4gSpP124JncMZNmAnwcpLeyFR9FCJbCz11ZO2g4Ui2LLZMBvpsw6RGk5rhe9qG3KvCC6CapxjPQYAURbnLX7F9DsLHTliJHmrnQRSGRJDdnxcEi%2BEwjrIAaa7rAHTOjhbcS5bty0%2FTRtECIRWKF%2BWUw4yxnRJp%2F5MIXUGKAKr8iJZ4CzBQQEN4b%2FPQsIiugKWUUhdasTMjGqI7hkBaeLzWlrKVVrYU%2FMX7eC9xMFd8Dl8PpwGWvuIACdwM%2BL0cEoHPFoeIg0ss6YB5GNDUI%2BVVVxlVQbiGzHht4SGsX4WyyittnhE1%2FbHSW5xK7dGvZ0J8xUGiEHb0uI%2FkOznvmMqPpq%2B8bpQMoktam199IrhjVSNE32gt4yMiXJQ8MgqpBZDUMOCJNEHHb%2BrRibTINrkJyyurAFq%2FJUFj041bgoPqXrA8pzPFWyQ%2B7o2NyxYgMJDjtsYGOqUBiK8jtQOFWhIwm8yPK3KzyfoGI%2FdiAhCRZcoovF6vYvJloApEp5Sw181eDNt0WCDl5vw9a%2FqtUkEGIS9jckDVXWvvY%2BP%2FwHZRonOwV%2BVTnnIHhzWkAc%2F%2FHbfHkTMmAUfP397nqfAUTcaKRB4mffbVijQbpAdaH56eOUXZowUhFaC3lhIVDY2r2jbphSoA6qvdYpZ4Ab5ssU8obTwiAtY6mmFU3GZw&X-Amz-Signature=582b83a37349e6a0d1a881c86ff214895cbd9c5a11050138c23c47b9e3867735&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

