---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667A56A6NF%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIEjbeiph9HRwisYM2XOx%2Fu7yhZ76Yv7ZPUb%2BsiGDsOHOAiEAuPceJSpcPGILanIdkvPW2wI6UlOfPGQWvOqqJVn8qhsqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNAa1qGy9Lf4jsz%2B9yrcA61OqRlQBpvJ8Ex9NZkHwT75pug%2BbCsHW%2FW7%2BwdMkBZE3d6JUz8mZGoHh2rbS0W585wXbkZPtWavVj2a9uPaf4UFerNaQqOAWqw43dU23ZJjXtDUSJCCgHWbDW6gCU2SxSDHVncLhO9HSQsGMqGuCSFHtU3TKp5qX4fNGjHzIkLHGWuvlsrsBmJCm4zSKPGov1kHo%2BiURu0VMIiOrOUE8dCHW6VpwgFALd72iEVrGw05sUb0GBj2lWVvq9ntRQFYc1b6fulrejL5aRT3V4IrG%2FX0ENXq1bKyg%2FcKIA%2FnDDV7wZV7J9eqHwMZBiZomA6Z0m23IZWJSXJ3s4TTPVpWk4HWSsgW6QO6WNvFz1rcN1Iyp97LZC7IduTiO1LdotdMW2LNpUt8ZgcMO1U8eUkG8qdYBKeM6Xhws1wlHgfQIVhmkp76ZBP16Zady3nJiTcjdbOw8jGGNIXZEtr18xrBQEPp9gL7hgmdaZp9XUehMCCNbTWsvyj9020lG7pte4%2Byz1OMkq9HpfXvID2w%2FhYB9rj3MDGNimFKB8xvfEv9sxs4cLcWRQoAqVjnOV0lh%2FlVIPdtM4C0dT6sozv2MxtCbFd%2B%2BOr9xyYM9GB5%2BTNnOwe5lrqxTMfYBTjb6J33MKaRwcgGOqUBPt0DsGRXLj04HU7JtqQPfbcda3twjwGzSkm73jfwK0VswK7DUMU5%2F6v0BLM84MRm33pMmhixozO2Cyg%2Bh9PqmUImtf08cae2oYSuZBMShCrT04aZKhTZjlWzjuOGkLGeP003BGYiLtl2h7Gu1S6%2BRdXZmEDGFZvYvnGhHEgz9aFjfSkPNzWwZ967p7aiFDlfwnt6SP70whn6HM%2BDG8gECyEWS5Tz&X-Amz-Signature=949abfbf295e10df49417449200aa783572ce2b492ccf98162ab01b2500091d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

