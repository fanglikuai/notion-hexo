---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZPYGU2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPDrF6iaJfWEY8YhPQ1%2FnHUNZp9eW5gGjRfrXG8e5qCwIhAM9AYyyP7NdfhD3eXgO5rJD%2FOTzJbK6CL%2B%2BqdJI9z5ixKv8DCC0QABoMNjM3NDIzMTgzODA1IgwCvuWMr77dBdSsgmcq3AO21vs87dfYmpWaakLSeaY67u0j73ihpxqVvrq7QU4Dd3FbH2txVVHI6ZK8uxWrqMAVpBHPUEw6mx7itfCZwbtPmE%2Bqjsl%2BF18trhIGuf%2FbchAysxeu6oN5Csc3NnVJkpuTpjeDgGQCvk9ty51tBPfg0BRubabJJyKqY%2BOoQRLGGfXBll3G6hkuqOQnl%2FA2yNZF7Mc37Q6xCdcvbjr612YP9%2B9FI6qJfGm0CUw5DqrNwuSzoLyVRp7a4Yw5jt5Vk6rdvQPjlnchqIrynbv0N1NHpMUbk5iQFhmacY4OexDolchAGtylgkjc%2FUdDOwEKUnho%2BQ9N9xAxuQjcr9KlrMk4WLrqQ%2Bc2xHJHv%2BKFwQPwY%2BkNNO2hxTbX1jlPcJzw%2BWe9I1renwCBbO%2BJMOTxWLJ1egmeZMp4JiFGw6Tq6IubywCOeGCsw70Jvq1pU0F8fGRrPf%2F%2Fo9Szkly2Gp84gUzYXKwOoc8EV9qJ%2FyD1z89Gr3Z5mQHt5DWuhKNXfvvgmBEXCbtGHZfL99nP0KHs04fYPJDCPTUG1Y0FUZvtfBnXEs8xjT3bZAu1Q6wOP7HZtfGXN5NfGB4VS5PS9%2BkaMPYPYaUD%2Ft8LMcHnjcMgpGK9xdiCCCfA%2Buw6fI%2FZojC1w%2FnGBjqkAdiyzjDnZmwsUopvRvAobR1kK5kKLnMugbgHCBf30M6RGSOefa%2BIE4QHYYysjiRuzlv1qUw0oF7oEGOWGGNMpFTD%2FNP3coLSJ31b%2FXPy01b5A5gzAuIBWwLTN2%2FiZolOxbR6R1a054twaPUPqaEv8fHVjxPrZoXuO3ur7iFkeTs5VPKYilO3Yx%2B3%2BB%2Bd8WTaQqLCi1mrNDflUhcCranWwbpHM96U&X-Amz-Signature=355103e393304c46c5f478c8b7ace40125fa87f3313b17a84f746169ba206560&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

