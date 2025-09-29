---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDT4XG3F%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQDb6dgQbTSqGRVYtdUmU6m79uakrA689XuBAz%2B%2F1%2FwJjQIhAMkCeLHkI12uFfZ8eA0LPSG8sr5LRrrb2F00J3Ssx6WCKogECN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxlofEgfblPloQcLhwq3ANGFP9DiSBk8YJvB9%2F29jjtKQFYJ7AixqhUoJP2O%2F715No5pgXRHTOhrnrRLsaraCSFPI47paaC3KszTn6JSagO3ivRHpUB8pTL4QVJtprFCjuu6zDCbDoS0e1KnF6TYIWbD6hS8Gqg9odkr7t8Uc0AxXkOmI4pp8%2FSftMIHfTpJkkt5NXbwvSQhGii9tMwlyv6lE7ttTqRfeOaPmdIWCAiyWjAbJqZrTYUHDA6doSmEk16GqXeQOWKtrCP45qqx0A5c2ArMKPnOZURWEHgIc%2F%2FcNK15aBoZwIeuv4srYJC35qJuXc0e2jSs402F2572cUuPg5B8qmvyE8aJKLAlZbXMPhEoUhIx6aG6CiwxJzsDE2hviAh4rt2tz1vt1zim%2FdUSmQbf%2Bx3fHQTbmsXpK8qcZrm4FndsYU4DPK7E%2BVZ0ak%2FarwbEaHmSgmipHkVTLAJF0NJYWkWmsUezmWr5mn8EryMJ35dBA04y0Kx4GCKd1GDBFO2f%2FSdDaff4WTAGXKpisVKaqCSBRzKZIlB17AI5gzYkjBWSDtdJo2Wcj3B9EVtp0LUG9VEYwO5US%2BPW4h4NfwM%2BHUufwtI9QWUbqRE%2BiWrg%2Buw572QzeeS73La7aHHY17R6znDLPk5TjDkiezGBjqkAY3AeXvf2YlsJOzFUeyqBL%2BtjbT9fcETNmrK9A8m%2Fv0VE68SYAAMauW9pGWwtHLFZUrA4225HmuoacA%2F5d8%2FdGFa%2FzXzUKPD%2FcZOq9HAsfZcDCZwDZgLETzfz1hVf98oczNc8YKmWvlBmvfXQOlDbqV2oJSGHAvsH%2BP0vsgpAYJ9kT7B9S9h3otGcuTCXkugQHO1UMFoi3ro%2BoiKZvaOxOf4uAPh&X-Amz-Signature=a571c30d286947cc8e8587adb5597ecb0ec53692e332d87a79844404215abcdc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

