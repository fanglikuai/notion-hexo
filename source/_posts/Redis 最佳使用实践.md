---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IB3C2LB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDyrrUHtt%2FkETswBD%2FF7OntmaAuAUKAumzZeaETTm5ClAIhAKUzn6Wq%2BXNPcytZ47aBLz%2FEWrYoXwh8sfsfTAb%2FrohiKogECL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0IGdQKfSMbCuRlDcq3AO8t%2BgSa4hheSTBFgenQ39rzGhVbVJJVsCpXHvX3zO95Wdg37c48LrmsdGdax0MGiZmai63fE0Gnwr3l7Ri5tDfff96qZIqTzCFhZ2EEf2XmlvM87q3VqqtqmG%2Bf0ICtbrJ7Q1HqRFZRqznGAQG1ydKv9QmsQ7ZOrYc0iSY6GU8lEG4CY5QG70xwLhpardt0Sc8b9xTBfAK%2F9gvjAPsBQIkDuhJKqrVWwEVC6khusgNaSCTvs6pWxU6crMmBAbJRx6IiAHR%2Bw3xSJUYhE0RucL2n3GqQeSL2PGY%2BkHIDhWbqJzSJHqqgL0BlU%2FbqOYlAc22Ihsva8LyYISANSqtPpzm8oV2eauEEBNx2PoEtPDcRb%2BdGeyBVwCPN2qVPNWAaIqSEXnkm5KCiFT2B8VafmNQwMPw65SrNDg4k6V05Nxw4N7Xidg7RcSSlJif0i9%2FjhfYTui2lhBcM6t5j9x35YY6OGn7IT5Bw5ZUMeNSG1eoLrRt01FBRK4u18PRKbnBIzdSyMTi9jurnBphdLpt5Cj9m%2BvXsdSEYWHvmBSGVnCviyBmtToWs%2BNRsWoXESvl0Xz%2B2SWARbKc7qV0YlSrSfOn97F42IQXM7taTOSk45DL94G553TcSUhfDK4nsTD42pnHBjqkAdnZmbt44vWujK5f7ZcQxFUm6sp%2FV535lTLOopWxzBmdUzZ1Y2bAODfZo59FhXGNOMRdaBprv9cQ9bESoIJS2ottl1fEBh1PFEqKBYe%2FumBZAt0BsuGx8LRfvSv0g7wIF49tRZKIHhQIQQKXIw8aBaejVRG3tANpbc7nwwEJ6nmN1tHSepQkrwTqEO2JNyM8QQrL4Ac6cLiIXEY9k3ROokZ9hXDI&X-Amz-Signature=cfd879a58988444859a5c2215624bdb6112da28ccb745bca9dad6645e71c3b47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

