---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTRAEBY%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQCc5cXmVBhUrU7gNl2YmZU5AoXOqvAdyCYXnQ%2FLegTKZgIhAN434K7g%2FekByjzCjVdy6jPyKHq9UzgZ%2FLPf01y7I%2FwqKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxrSFpbEWTyTOMRpeMq3ANY8yUQFC00EfvdEnFKOlwl%2BJrhGnrMT2jVQenF5h%2BbOqY3t5PJta%2BvZjcjJ0quHCcbXewWvshh06005KC%2BTwdb18kRDgVHdssdfzBJqkS%2BZ6UhVIbyCy7x2Xttm5NcfqGuqRgk5teMpSJ%2B2GOiSt2KBf4ZdzsILQHcu3tj1Vl24bOYZzuZ86l9N0TraadWP9vrDwqsC7AdQ8yT%2BVlo7lwxqEmIacHSTsrfJMU0v%2BBRZkzGdE5CvkTpnLHz3ZbzmV8AjsqkT1Qn9BukiPwFoefeaRPKX6UeNee43cHGg%2BBkQE%2FRWkoeRC%2BaKoz4RipvRvK8Zz48X63zop%2BUH%2BziODl1YHJhztV6OztTN7H2kl9Tr0N9swZzBy4%2FHc%2FWnCF4OBJhD9Yd6clMtUDl9k%2BGeQ1%2BjlsP1xJLCr%2BuSypQa%2FLe8ZAE9ojAxrNRyODpNm2NHrsx7VYsx2PcMHiUPorXtKc9cMOZuT9sgWgsdLLnasez%2BjvsKMjSskCdQDKR4T5BNzFYvFEQLWE9Gz61sCxcbgN9EvqY94QJy64d7Ev4Y13eQLhEAM6C6r%2BDZwW2z01IMIxoDJhjVF2%2BUTyIIO8QLaeXG%2BHF1DljkdyWb6JrhI7T3bZZS8lUbxweTurS5TCk9%2FnIBjqkAbO0kHbullT0GxOJ6HNkl9owldm8XPHnGG4IGqQWAQSLpx8zm%2FXXKus9h3CLCJjLXLVOG51DcxMeLejzbaAUs7GJm5tky97tMn5aKR8V7ulz6E%2FDOhGtjqkB7QwkkVz58IGGSzlrZKyXJB4VJoARz05jznDE3zcAP1OLDDH9m8RiTOYR6oeTTGPc%2F%2FP4NW3%2F8Pe95%2BUyTw5cS1vOgkbocS8k%2FPgK&X-Amz-Signature=420e6fd476b9a7d10c5f61e107c211c2b48ece925b6a64d374e283f002ed350c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

