---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EYMOJJW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIBABmpghBO8x2abjNlrfL5XauG7w6TENFmedTbQ6ZemrAiAPwAO3eh99qe%2BtcHKPZ7M7qNN%2FfXPReq%2FxKQ7M9uLQ8SqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWuwfMrzqFGg8SgaNKtwDAAm68rJnU1uUjHm1vBF6KOJ8nmudcq9gbWyb3VakjjqhHj5jnfPqeYz1PNOkhQr7MDBJvtpH1JGgoZiOXp2IlF%2BLmOm7WOKG1u7iB1%2FyREK3wu9SVZ6imw2QOMRk%2Bxh6ehUrbvD1C7JjVl6BrJhOHBMQnRQMVAJmfl%2BqfBTJkz1qb3iWARRV8SIzVH5GdHZerCr47PH5e0ao2OnoLk2SLoPmidATCqeywOZd79fIjALgXpUFmSN7lBYULQj20wXsmfa%2BDVWvcBWypBNgDCJ12elbUD839wQeoC3iDBSmvvDp57XzZvK0M7M%2F5tTnG%2FUx81C0Q7xJHW76pfjO67aVqBGxBf6i9XTPy%2Bme3UVohFYMaamFkR%2BIbOv%2Fb7QELVRgfhAZUDMegT77YlWJHIr%2FQFRnKN%2BZUGQGi1rYGE%2Fns0a10Z6fDX%2Fo22O2mdmWyCJvt7afc8jGDcIfYldhCWITfCdpw0xEUHQRrAyBxehb91TxMOZIyuIOZiwqXNPMN%2BhZwikp10rKEDxrCi4OiZiDRMcHGLULRToa%2B6Ju%2FGh%2F%2FKXnaKCpL0bJlg9WfmLNxA3hwPne%2FEjKXGncNZMUbKjdp9etntsUMUm6JmUhhB2Qz%2FtSzyoNe5fxdsbKLUQwp6icxwY6pgHCK0EIihSj2pbd3umq%2Bt%2BcyiFBdiY17urQPIrByXavZqIRemcX5%2BvvrTPoebsPqvwC3v%2Bw7kateaEO5OSWa%2BWKzx%2BT5JRK8gGv90T0v3mEZ5Xu5aSYEU1GGm1tYBaUeFZLOtnZDVC7gK8zO%2FV2CiueOPfjLpOpv%2BO%2BArSUvg6UTInVz01109X2pWy8wDHjSG0NNNvmktrC4Q4IUGUBPEPepfbTgx1Y&X-Amz-Signature=f5a261dcbd23818db1cebee50720a55084659eff2a9f5317532d006f3c14aa72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

