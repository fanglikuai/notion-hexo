---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYBGGOM6%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIBA7VbgTvAaWupna%2Bj5HPBvW5JzVftOpuXowVg7r0iY5AiAxrjW8RlLn0PfFgU%2B3Gc9kWckkZzK7DQYtEJ70COQ2MiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlYZUNLLxwWGO%2BjtUKtwDUiO4YH3Z7ArgHtqZlNRP8UN6YycMMGXJBW9VQdsUkGYVRlPAFk4tyGnn%2BzyaUjrBg9JAZhAmTWdWPiGodJ8IkpCoV4EeLqu5So%2BmZYj7NXBCCSJayAjEScTCrUZW8oTIxiaAq19aoASQqJDIljuISGoq%2B6H1%2BTb6kvaRiL5KOh%2BJpAh6b9eZDljIbcSAAfm6P%2B84lAOulrmN6%2Bl%2BjuIlfuJhGg%2BfSoKr%2BgRdrrAiXUr8BzHuIwrXz%2FXqlg1%2BJvht%2FJ5PMT1f36QDBZQePOF4jYOraQRXtD38nWI11eWXhDFmEWgRsWfssYDvM3aOwGC53LuofgZucSbZq2YuymgbGnhikJyMYqOsIt%2FaU9nGJZOq97rbXQshJtsPvVY6g%2B1Qn4FYkk9e47V3ZmZvB%2FYA%2FljaUsbeAPsvreMg%2FkoGgxsDX3NnT8FRD1a1bp30raApN0NThZpo23NdChQT%2F%2F%2F%2Fj0jG1Ugb47wbkqG%2BUsXPoDHiDOAJDdPOBzlRVXpa3eXQdbJm05SYck4cxegIyHSyNjrOc6%2FWmfBK9jTVQqc%2B2qRJSUpX2jOWCexNrXeZ9NiaWS1iEWD0%2FzcKN1zFEfooyTxDQwzEl98TSwutiDibSskdtJmSQOW0%2F9jXoD4wqYCVxwY6pgHlmbNWcfwsBYs%2B34H8pL4elXgWLri87i0faOFgoIJC5nUzesgPWuHooNz%2Bvw2EsRk%2FtMemDbySamOWEx4fAD2miTim7UgLZ05syQBmGI5%2BCU%2B6ac06kUC7sOT%2FvtMjKkx3RVfn4Y0pXPwZP%2FI0mYUaAaqwjjcGAaaakgE5Z5Z9jDAv7RH546TM6i27foyQjl8FtUhyofmHvkBxY1A5GRrsP3HT9uZW&X-Amz-Signature=8308c43cfacddedacf5f6c0aa30af9b2e6145742aadec2bc874ddc27dd03f32e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

