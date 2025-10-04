---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X263LBGX%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTd9LhymLrklMJYCUjGk%2Bv5cbFAibKUcpUhlRsmvJZEAIhALciP%2BwjmAYR5slXSlXIpvW8f4ixiPVf0jO%2FnbU2tXS0Kv8DCFQQABoMNjM3NDIzMTgzODA1IgwgOu35BjkEbCydzwoq3APs0hN2aM5hFufJfuRJQNVR2dPG59a0AShTgy2fBhscSSyQFeUu5uwdQ0pGTTWq8%2FtDwSqMoQy6DIBMbsWPqkAXRkcQXfvvSSBVzNL%2BZzikxvTYZ6RG0LdfO25LjyD4BgzIXCtpTeKfOZuFCBHYbnMURnYwkJjkxZ9tbylyDG6Y13dECWbGq%2FpafaS7Xs%2Fgy%2BxssudRbaweEFMC2gWMN7%2Bt4flTcQThWWNQqv%2BYhIwJjVGK6O3fLaTTMKa8UoUbfIDI3d91H2Y%2FdkOoK0MSS%2FDYvzxIZnFOfGiL2%2FHQNLTmzNKgaPNT1bcQQoQ7Fjvnf3VUPW4t3Lm%2FMEygh9AwtN3IPooKtZM%2BOjSD%2Byy%2B4AgmlQYHhyBuZ5C%2FAwpCBa7%2BDmqAjaJX97W%2Bkz8XQj0ZP%2BUeV%2FcVE9dWuvbh%2B8nObhiN7v0nzY84VfCJFrPwS8lu%2BulkuO2rJHzOasX7M4cs1nyqVa9PNNw0UTqWAOqE4i%2Bi%2BNDyRF%2BLvFbIO2z%2BzaG3bNNKHztVmPL%2F0ZxUqpx8Y91RV4A8HozXgGV17MRPtyq%2B2pzTcdKS21WPzpk3gy5YwAGnOTmMboSSf7DuTS3rySSD73n0kQtF7JHv13LqXrhKJtB87DF%2BbjM01EzhHDC%2FoILHBjqkAcO7p76TbXGfn1qWKjlK0k%2BiXLDUwzSQcOtLLytxlvCIjjrW5vOKJCQaBZhErM22ksYY6rp8ppZCu7iy2X5i8IH3FF6Cc3QjKwjKT7letqiljjIgecQzOfRatJq7jvsUBWVnIilyK5jKRd1Tr1b8y26LjwCJkTEL0Z9E2d9JSTEGrHbpluHDYuuZimaNUgx8PJPJGfJZmkPqo9ncFOo%2BgYTKdZ5F&X-Amz-Signature=ac3957c02a9976d97e0f4370c9c70693949e4acb600fe370653a377d7b2a98cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

