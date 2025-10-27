---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YEFRI5X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGbmmsLvEzvsKsviuV12PYqTZ33CcOoAHoZn%2BYFyTFxfAiBmBoYFyEYDdEe%2BqVzDqtzbgoVP4WZRvVC13Xske3xBgyqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTryColROW7A9TPT9KtwD6VLRFD92nJa8H%2F8zkfL%2Fu6j3k03Ej4zLYCP1T9J4U5ia3ACryVOqeRHZms8p32Kc8otjWJQ63H4K6sItDX7PSXRx5DAUHXnhr9uE%2BUyq6tr7q3gmMGeJaXuMJxdgUZKrs7s7M%2FS2j6hSRx2hkliuUQWYAhK%2BriHjPsmBP64S72SBQdtU4iWugNFPW9q6XgCJ2Wt7MvXixJCJ6SVoqyWqqlzw%2FHKkwPDCxVIkmwY%2F7K6CxLwnDdHG3PQ5jcZzK61riBJ3JgpMu9ZvtCw145rIBE0GGYp1iS1X9QsJ%2Fu%2B0ZPbvkSRXgeetzjVwbcF6LPLB8%2FWJeyp%2ByhDjvqsh7N%2B9xu8ugf%2B3375W4vbJyEOOWemBQeuNajFVQvaawkY0UoY5z7LZq6FnIlHV4Kc%2BwbmXwD%2FFKbfNCDK8k2ush1Hy4zX9lb4yWN%2F%2BsCAgwgBLhxkEb%2BQKsMqPsGO0RB5%2F%2BUnGYdTyo6YIxwKp7HdrVUisD8SQTK2iVLB1qrr3Pb1msVeyOFP5OUNGOMxIqDBrrfINvI6lq4e7brdwuRyE2oB2T%2BzEJP85lCm4o4qrkSFOZ7YPU6mycRJtd9dHyg%2Bws7PjAvLomSMosxoDJmPmce7Zg7ZRSuxH0T4YyS3mr3Ywmvn6xwY6pgHocZPCEarwd3hVPHdLxoLk7ufRj7cVOAI2iqHNkW947rgfqqmI%2BxrlhlhmEdT1JfMe6ox8B2HJIQtnVk2OcgOKTTLKqzHhLFELhGHRp8jlUIug%2BgAe1k%2BtMVAcmDoKSUw5zgKy1KrNzLKioyH8Hc9MeZjGI4Pivgd8Hf77ZSPSc4HeN2FP5xKC7PjQ6Hp1P6sLDF6nl5vFdpceCpl9lXYXE7p5me1q&X-Amz-Signature=4caf1f276b5000366c2d4846a7e899f797f5814c4b2874e8fd25faa7b569ee10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

