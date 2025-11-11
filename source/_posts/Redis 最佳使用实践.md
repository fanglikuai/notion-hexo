---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IGJHWF2%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJGMEQCIGqrbsTqAD0FHJ8C41jaW2E2zdEanJQTukmqA6lwNiK6AiBbwRObZYn3ZGEbuRxIqrBpG0RwkZVo1MkbBJBrkewXgyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMSomKtL85yDrftt9EKtwDZOX1ulUkRh%2B5kde%2BBbQdAHqeB10jb6hvG5ilheeMz8nbMzzKFMVKOxkP%2BeqENmW94EMd3IRb7LUBKVKobHwiouj85CQul12%2By69mj1FohtZ9dj90nz31bFZfvhqK9NNYQR86BmYulFsvZfubxvOmuwVvNpKLnueujfVqnaPZJ9tC48hiuwxXwCy%2FmWaHDNNMXsAHeOZdbk%2B6JDQZ9JlRdBPRF%2BjEWKJA3K3FcRv3CLKWYM9UN0NVSjE4apew25rKni8n9NOoQk4Ln%2B4n0yvueGBRBQsxkO1Pypj3GbUMzCHlGdqx9BFjywh0Y7zZWseWryZjD2GNx3Xn3SjWG6mYGdFaTTrS6Zv6xDCgkZDmrf9oVaQ1mB28l10b4m4cUn1mUmbCKwwP6vzPZL9zSgBV7eKAWhPTyQpjrijwjtL78AgZiWNAlKt9E4cc8IuKaqPrX8dVBVb0B6pbFtHDqAi4xWjc12g003LP2be4CeBDMS33DI3kS%2BuAzLfUBfem1IH4ptPfzTg7lYLfPzZnBK%2BvNth0v0AohANMFlnkwjB1PLZXP%2FhEZ0iajHYqdD9aF2pH05sRYDRipJaGArl8c6BFV%2BxNr4TcqrihdyOZThQWKV%2Ft8ZOB1jzEZNDaMJAws5zMyAY6pgHSbWEC6TwAjtU1sUQpiK6vCE4J3RE6WrtLW4KgWjwHQcrmzSkrTqd2JJZdGUu19GfLX8kSaTYFMELjlWTJB5OrODQhIiBCY5%2BOdV1hGitYImaCnDIh0ETqVrNI3MV%2FbRyujdFqQ%2BqZB8oJRI5FVN%2FejxISsPyFTxGd2OGDs6WgvFW6AS1bsM7506p65BpUaFKmwaKLGcTGpciFRktT4pmKXQA9IAmO&X-Amz-Signature=5e144e0e6e6d06fe4dde219ec86c38f35a62b1520142ca22b02091876c837561&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

