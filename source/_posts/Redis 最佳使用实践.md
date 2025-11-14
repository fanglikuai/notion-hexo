---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XSAB4U2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDI9RXJvdszKaeROdc%2FdKaodqh7WdTVEcVggL4MLhIajAIhAOjyWRoVwbQO9I5W5XbYkfV1CZaLZcWJUvbTj2kHFAdwKv8DCFsQABoMNjM3NDIzMTgzODA1IgwK1gUi1d5MOyhPD3cq3AM%2B76NjHRt1I%2BUmcuImEsQ7Zak2GknV7z%2BcV1yfe7fVOJghi9xaoS09%2BfKqSHgrcfrwRc5JSRYH8CaN8a88h7Mr0%2BeOrQV%2FnoXmNLwv%2FNu7v2LjjtdfusqMv02mhANnUcdz7l6ztHQohg%2FZin%2F5IjbOA2Rv%2FYF7VN1szbtEoiBHQmMRSJ8%2BoQI%2FOCh6UduaC%2F6b0vC5iNLhVQ3iRb8067p1jOb0A%2FnfC5fGjnzGIvto%2BW6IlU0OW2faRZaAjBv9YMqNedB%2BUUdUKz97dDySovGOW1BRpGQ2XDRFDygCTMExJEpf2myCbMiO8dZrhyI5NiGkTEJ1rDfaBY9jicfDk9uDHOXDl1G36L4NxWAuhbMGvvwmJB97fPQedf4bap9AcFnu38CfNl6d7qKPycOhlQNcF95CCoqvXxzDbkFh3Hg7xuqqEVs6otnBbkxRLwA3P5Aq9xgr6%2B%2FIijp03%2FP0M4ZebGLaV2wCXA%2FYRxgNxt6I4cuS41Rtxzd0yMRokmlLW1z7CADDew9FUQlUKiaDGRPFjk1OBXZRjpWlguQkM9V9UoydbIFlZsJyPbt76dC%2BD8qeq2xXRHzN6TPmkXhiNKzytsQUvxc84XyohAYFxa2%2FCxT89VI5r9DkS%2FNClzDjk9rIBjqkAcdnXmpY2%2FR5r0zDJtICk1x0s5lHR0AsTHaguFQSjzS32IQ43OclMIbaogezN4gL4%2FhA6pyp4jrsiPTn25ZIX5lUaY4wqme6EVSLi64hKeMjVsSTK3NwTtnrfl5y94w6%2BTG403bS0oaSpUZ011f4yg8nKeHaDGQFY7x%2Fuc%2BaP%2F%2BSZEhPmo5N5WQHrZGN45hri0AjmZ2GdAOUrOvx9S6pEdnBEdmh&X-Amz-Signature=a39622f6216463688a723c1bf91322bf4950ef7af7a6793ff4516f9256991d5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

