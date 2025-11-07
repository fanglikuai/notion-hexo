---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGMEXELE%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNmcqWwnNYbPLeea%2BvpuaOWzQON2kBrlchvimgmN4WqwIhAJp5s7UTUhGl7mkc6MawemEYEY6iu3D%2BVrFTmYLEi5MvKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzgtc%2BXhm0%2Ba%2F%2FJOQkq3APp%2FZRuI6BdSMkmJbjSleblPMzWyFHty0GPbRx7TnjAgc2sK1Xl9a2bAEjyqkdstvDW5nyzZBkaUpFmgc%2FcVwXrKoEOdFZAEOMnE1eif1ug1uGYC9OnfZ0116hAATY6Rr9gjFqpyR5bHqqnmApXan8%2F4sSfsvyRUquhiQe2oJUp0Or2ZqOeutsJ69iMll36kq1tA5LfCRGj5TW7h6og6XMRpuB1QjxtRuZC5ie%2F2GyAQzH5PIf0lgKI2hZ5y93EsNx2mw%2BKDmSoTErxgf75WMspAU8%2BfiKsn2O%2BUCp84HZtwvjmTERPSlh7q%2FoY8uJzrVHfug5RKnYiDxyWC6QZpTIk3wLz%2Bywow99%2BHEc%2FW%2BAC0snCuMfcElRcDtZKN%2FMCNtqo1CSltVQKSvz0OcuCZF2EyXnzUgpOeNwUX7AwmS5u3FHdMvV8v79ZRI%2Bnq5%2FGhWYUveHRetXrdh1zYjjHCT6729AZySxQuk1oXgGyinmnxDOhwCk1WBaop0hHX4T3onx0pcn0YJlZrYo6jCcB3KauPnGLCMrL6oCpM2pCXjvzdYURNjISnZukh5v2dgNG%2BVy8J%2F2F7Z28Ifj8L0CDdwd8GuPt916tF%2BVwNAVJXl%2FIvUxHzpzEn6nYS4wjZzCSnbnIBjqkAagBT%2FjYiG%2F5dTaSrstjNXRzVr58hDthDRxokW5GjPu5zyFDWUysuwbZO2QRgqh3LUz78jFwXveSMn0Zx23T%2BdF1W5P5t4%2Fh084QA8WCsNcH7vCmfYHWZb8eoio6y%2FCG5p8cgMtXIUPjeJSN1gWURoQw93pTmYDJBgAVQrecAredRhi%2F7MVpKbN%2F9g%2BrlAHrTBX8tE6jCrQKrQTDtrXa6uecrqoJ&X-Amz-Signature=5b52d846e668e18f7f1ed7dea7c4ffafc51e3d614d34fa6d4d393ad5e8b7ac85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

