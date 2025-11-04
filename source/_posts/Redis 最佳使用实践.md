---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYA4SIFJ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHSL1s4Qb4rfZYdo%2By8OGD4RLHTbgplWeUfgPHbfSRn3AiEA4iMhESZtATnerxn9g%2BxmX32PLrZpL2iDZ14ggRsnYNMq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDMTxTAycHYuuGIMqfSrcA%2BGI3axjBtLABn8OW6YaOmiG3GrClg%2BTpnsJroYrD860qWjJCIF0iqrZN4OobofO%2B5hewoONelLYwh%2BI9ZWaL8dVay%2BDdEhSp7G%2BR5UGap5%2FwLYvfcBq%2FAgMZC%2B7H1mXdm7NddzEwI16sKqHGlUihfF1606Z8raKUdOsAQUFN28lJG2lQuYH9vGVpa8rpCj2VraaA86TIK33rkYwkTUXgOjebqQR3O233lkvMcIp1c1cIIQcvJguSqrSGWbSwqZmsGzDb80aOTlHsuQjbANYEDbcy8j9DaccnNgvEStKfRglnH3sGZfGHTczL63hLjWkqkYNWkfYwJZwnSyiGNpJd9it5qkGdFoWaS7tMDZicwverbluOFZfmhNP6xaVVm0AZevAt1EVNHMU%2F4BocD%2B3qc4h2AaAuHheCwnF1fqkZwJpuJetxPYyAsUXhrzFSlOrXd%2FgoGA2ipnAq7BuyK0ybIYcFWSfVfJ2ChvrZQabbiax1kT5z9A6Nd0AVGNdPyX6gWPFlSyE6eAYTCDIDcXV4zPan8vlLAAzMoKVi4l%2B%2FosnJNjXjj9UTs940uqWwMlTTAtEEMN6wR9kUhoPrm6QNHNhyASe9%2FhupyW56dXRx72zUGNt15tV7x8pu0DqMOiqpsgGOqUB8Jvf8N3kwYrbuVeHMsvVwLoescPBQDplK%2BCsGrdXVUcFXoHGToAE%2BgjeyEw%2BU4KUX9sKDOezW%2BDJSW9HyKNcM05rn0%2B2LQ3vQkRa8KeRJ1wOl1vYMizDdEd9vsB9QX6Hht6iqiFhxhajmeJpgQI9ocxH41rOSY6s8E6jREStYEedt0hShjHUtEm5Mz3aOkcFC1j6SFetaTsNO1QAlmFYiWs%2FYOQL&X-Amz-Signature=bd29188480c9da0c3274b8e3d06cb9135a8b152d45cd5885b8f1d1879f69cdbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

