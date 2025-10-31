---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUWNDMXZ%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIGcalp7HaW9boQCXr5XYv7fAccieDtgu0rxBo7%2Bz2gyTAiBI7bifevhq68Lm4ma%2FRAWWHEh1dZmFiYfzhfIW2%2BqlkyqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BvtGXYObtJY%2Bk8oHKtwDSzpkwYoSpWrhRo0RgTPlEZnlCHOvP%2F4f91I2x3IocDZmw6wXkRcjkHRiIVaK3oJGqXeel1FJTrvVyfY7HnIvuq6kMLUPOTp9MlB%2FjC20p1%2FQGFezebIi7tRsNxhQAMPZp9m5ARDR2yRcOtMYTXmyJdG0zyYQvCpobMN9GTb5szvK6A9WVSSpYAkSSDNax9a%2BTKQMmGBIxN1qBPjq5TamDVTSbNoWAnn7VDYEhgHzZItb5ST0Kz9owscnB9bx3EToeL%2BRNzypAVmUh9teiOiM7coC4yA2rW3OWl82NjRsM4%2FnSoZ4UdyJHkbKAPPmyKCbrpB8r%2FnVF4NGKmPmO3eTp0BHGaG7sKxSRPgOSdu59v%2BK%2Fxx4xeMV1ou0yBc%2BIym392u6vFD9fjidnFGF27V5uMbGxRF5MzX%2Bg%2BGo7R7yDIy%2FnGcu%2BmLJn9BqQHtFdqn%2FdpkqX0DBp7bkSNLgsluxr1LnH%2BEQNnjRKv%2BHO39q24ZFYzg%2FZ2ZUTWDe3qacr2TPy14YdmzZhrw2aqiBp9uVF9wvRzYYePXjWqVRv%2FgnADm8CCL%2FgN9%2FD%2FxJZ3V8DVuCSg%2BKUJ2%2FIQyBTdhDI4EQyKJj5ANbSRT2bPkBvuPScmkMwsDlNcSZ3QskUFMwuu2QyAY6pgHpo7OqRy0uyDWVd5NmLoeEG7ADoQcU2xoiGEYroeh1GCdWfJNS4yOQZRnSyTPfTtWX0q%2FKFJ%2FpWXKN3Dd92cFM6VGMyVNfOkEQAHh6EN4Xv49qhSRONap%2BP4%2BFcr4mIkrFC%2BhJvTQumgyfF6bB5hTKCRu%2F%2F%2BjdafsjaAgp%2Bol33hKW4X%2FIba%2BfpIZCsUnoFWR5UKifbF02vl6MxV%2BiMxfuF8b%2FXYGh&X-Amz-Signature=622165cb040bea93a85d391f45c0c65caac18b07c9d356b3c44448b592781981&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

