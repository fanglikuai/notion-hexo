---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3PS7BLD%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T120136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2FBVmm2Z9YY%2BP9RA%2FKEIWNF3TWdsfikxtOrpfr4LRUTwIhAKQGlO1kHQVJb3Xl%2Bswo3baXXV7TOmbGjshXAq9BRqbQKv8DCHUQABoMNjM3NDIzMTgzODA1IgxTkhZi9U9m4ZbSCbQq3APrvNU8j7AH0bbOY3futwdi4JO7SKlthQeUvCDi7jk7h0SfbMABv9evXi1%2FMIsgTjeEh7PEWacy2K1VceuaK1GaOMpKVgWOtOBL1UjJCpc4Y5nKe%2BbPm0kROyrY%2BwVVCnjtpXeTsLDoo8ZX1caRK%2FwO7BIPkpEGRRu2zQtf9ymkqPKUbIVoa1kshl85bWjTvhVFb%2FpYnqulx%2FIlFNdXD5FYQi4%2BPbrb43gfG5madf3oi0xTKTDEdugsminhNzEBP1GV2p5FcclT%2FLO1BefhTx4wQi%2FLdzFVj42UpininNwZtRW92xASJVF8h6Ab8PgP%2BZIOaZzrLrYyVwabj1gw%2BxNx%2BZwn9xoPBYwKdALOsH5k%2FKfhrIJmiTPjMqjwTidxO0XiF%2FXnN1mUrne0jyms%2BnD9v5jzRz9K0sSUxKBpvtqFp%2F7XpJ29P7IMa9TuKER0mv7mYO9P0at3D%2Boh4Q3ggah8nMnnpww7n8iyKA%2Bfent2Dc%2BpIW2mlVUeZPLrPxtpXSmZjmFKVuv2fUVa4ST7v32JsPvIB9w7maMaYocKKcouZPoUk2FL79G1eeoq%2FpL%2Bdakh7eOJYVkNaF1RL%2BoEiEja0nfByjrJGgm1wetQFYQFFhe%2BazxCz2T%2BNg7uiDDnmL7HBjqkAYj%2FPSusprZ4UGQsaeh0NuZDmIDEKjtBg7prcJWWNN1iZYeIbbx%2BAuuqYWZRANAr0LgsoWjwodfVOSE45jJGsVsbCPZWDnudz7r1jdacaZwZy%2FA6w%2FNHRj1i9hIqpgZr2g%2FUVzyVsogmBNzICoL%2FX8tv9Zzyd%2B9UaZw64N2eRCVJZdpgOEntOEbIiKW7SCAHbMjaR2HmIExMwNbAoEWCcC5IvFXU&X-Amz-Signature=6291fcc38eef2acff3f9b194ab9f0e3c988216f302813c0d0e386c9f0f6f0b31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

