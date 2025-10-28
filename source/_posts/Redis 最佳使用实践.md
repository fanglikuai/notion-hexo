---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663AG7JXZQ%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDcvkWUjWd71vMqs79xzvHaHaAlzJSkbftdQmzHFqbqKAIhALBWb%2BZAVFH9YD6%2FsWVAHR8ZXjZK0MBxB5NhSS4fGARuKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhM4nfrm2EsnVPbVgq3AOSHK2MeEuA1QvNwUXS0m2IXhO%2FWlBR%2BNyunWo%2BAduNxd2shLCYOQwnN6Ize6DfxP3bT4A%2FOQFu4dUVhiDQYw0XvblfA2lKBSX5dBZT0N%2BQFeASkiMJyZt7q%2BjvgMOTHJqUn7qjsQIayHXe2EESM%2B%2F0%2BbFZKxmVU%2FyXr4xk7KuVF%2B1Dqx%2FhZhm33vdxdNSEr783483%2FVgcN3q%2BnaOtvISiKxnZkDpw7UqU0JyREFQGikn5eg2BOLuHM2Ae1WAmXW28GUOI0MBAOy7tGX9kZ0dmJXPb4%2Bgg6rrpLtA9AsNJ9hJUUHZS8uomXv2JyY6HNyq3Wrt32sjZNmf1TRrrLxg84Uj7PvcFJl%2BlYqszrEHB7JfncICF%2FfdKWrKo3ItvPKNPru4mykxU3kbhLdn8ScoX4FOpEKNtUpxW%2BFXbTVd0%2FO39ANtvzWBlAOgPkjzn7VcwEV26B4AlTBukY1l%2Fc33ayBF6wo70kUqpD2ed4pcAUY0iJ6R0%2BBnmxBCBzNEo%2FYLQUYtnUplQSfdWTGzWKRAXTR%2FYdDqjFwmk5Aiy8lbaNAgjTLLdybzff1G78NqPPi0FyOYxgnO5ki7cSGEAN9dtHEc2oaAxwpuXmCA6L93iqNZMpR5Cy80vEKdgomzDasIHIBjqkAX0gAfVsbmvvvyhOK2r%2F69hmyT4xkxGdv71I2TFRHlaFXF6M8m%2Fesd2RbNiN%2F0K21VefsJL%2BdM4y9M1gtrlmr2CJurw1wcbEbnXURzJfOcipQ%2FYg%2F01ryx3VhQNQHTxWDZW6jChlsvHyTnzLRSV%2FreR4GCaUNnvjKOEnm5nEgN6SjrwI%2FYVOtW9KB7u9bsQPZBksUexh77sEO1hV1t499lPmO9Zt&X-Amz-Signature=536a9dd32360c616621c0bf510d2a3264bca5d4266f6ca0c4e34854a73d73880&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

