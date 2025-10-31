---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624I733DT%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJGMEQCIG1WjY1ggjmWxjVYbvpxYhorg5FAY0A82KOCHxb2Q%2B7GAiBfwHwB5u%2FpHmU4ybFqpkjCoeKjYrQKqq92xGUlzg1PgiqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZWHlf7lD2wwbmVHHKtwD5LPIO6CpA2jyxbOvmwIJR4%2BVtik0k2iP3cDrBO9YCmHutBRirDuVl3%2B22lnYTeXDSVERW2y%2FATCsvqY3S7U8JOZztz23IlQeoWpcCbmrNCnokqvUxbmpL%2Fkw3j9VydyXrPwB6U6KmbpMtZwTPB7tz%2BkIeFrCHQtbOk3Mn23%2FVou756ArY36wzoxfY4NtYHHXzY77dbUepD0GvF%2FyeqmIvf8ILR6AX85vhVeaveIONs4AZom6y7dFGmbSqZxrOsANLKnZrpf2Knshn9xG6v6NqJskyOcbAJmqSJ9L45gO8EBpLehpZnkx%2FBDhQi%2B8xGwPErcyDN4atM1VdJgO2AZQH2oO6sEa06nf80zOoghNjzmCdMEVhHqIuss4Bc2p3UK7UhwsJPRbR3mx%2FZ68kpemPwkORopEnBfz%2B03L0izrvHYnMSg2%2BvAyrq2tLvCurWDXzVCYZM6eqLUSBmchuKa56RdOPEyZmOI0vB7zqqkcoVx2E3LDzSBxuf02oVd5M6YfbY0BY7Gud%2BjUBpkF04Fo8E%2FPKiT6JzEJpvAALMQl2GuBskTWCqwbV%2Bn4K6odwF5qVeBzPI1zAO0bS6v%2BM69P1reaX8b4N7axWVEAX0hxGTupdGkszJYs4w%2FXZhMw58iQyAY6pgHI6JyEbUW5Hw0gq9nTSwgg8ajWhLlTtzbgZajKZxXSOXXeJ83nya46mmU%2FkEOZ%2BxnpeA%2FRqQeVYeIbCyI6mL978XNmHAP0J4ZBe8ZGvoGZp1k2oTi2H6JYXY%2Fegwt7LCCMcQSkdS4w41u1c4Mxy2GdjOLJSOtBaPc86TzSYLQH7DgnieNJQtY3C1NU69vILp2hJMmAH43RnluGPxVFopzW1b9%2FwB9D&X-Amz-Signature=7faa6050d174f9fe55bcfa92f4d5ec0b98eaa8b63fea2186784f613094b8d1a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

