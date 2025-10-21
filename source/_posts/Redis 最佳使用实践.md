---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPNAVZHZ%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T190052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQD%2FBbZLMXTJjbywmXOHG3r3ZG%2BQaa50d44IN0xJj7AcWAIhAIe91UAzzMra%2BxdjhDxZLdr3vu2RFwVIW72RaDGR5E6WKv8DCBsQABoMNjM3NDIzMTgzODA1Igx8zmUBwkkTRtGgILEq3APHesxTx%2FHRtSrx9HQmGEsRFDhdtik5aaDSp8wc8I5Ear6rNWO3HEQBobdmW4QCGOjBiki%2BYsJdYQYS0favB8A6yBOg4NAnyu8%2BPo3IJfuL5s%2FvEuZzZGcqB6TKI%2BkT%2B3pVLJTMQwt8Ja6geoCIO2eMSvxQyb4jp7VHb7N%2FJqB%2Fa7ES6t%2F%2FSh5Kbf%2BO7FZYT6fJbI49egSOfBp5hG14YqJuyv5XIDxuZVYuZspyqJ1JUPLEUQUB60AQNHg0GzokLbD7ZZ0UJOIwL7ZP08KIQEYocPB%2Fm5gj3cpAi7eC5gMesJJYy7%2FUmOX3mGOl8HJUVBDGkStc4T9eLFvPrADBlt%2FALoXgRDMZQ1YQWYuuZd1t%2B81WqLFpJkv%2BNio%2BogDjFkBZvq9vKPF8bb6PPaUMV8s2BUzN5JequpseITj55RLcl1g%2F6oQjuDuc3zOLuee72kwisG0jzr6SrBBSqfi3KhqEfm2cvtUsWidvj6cDDgLeURCGfeCRwg0kWv4NGf%2FG2mjV1Z3WAumc4eZ%2B03dT9sqzuFJcpufVGNITLsbVHYUTsNnSWkYEQvnykdBBthygUbstioJNUvTA4wZ2uGoF7iPRFP7n3S4WGUXxA1a7fsp%2FIyWBlsxJQmqVIShb2zDlk9%2FHBjqkAaIKRNJPDChlQfGOFZcLsy5bi4H%2FtGfKFDODF1jlI6yL4IJEYshivXIfolmogooEpvKgU5%2BZ2jxuwElfzAO%2FiYyeYMNThiT1r2IVOm8TCBYO1iuqooKUscsiDbSUdJkEcBzrLzMM7%2FY6mMyAw%2B1tWTWuTfmoFGeD6ZcuHjkZMgg%2Fg2HOMYcyCGLCjnEisOvIM1ZfK9xJTvxoe%2FNns0QgvZVN0PJB&X-Amz-Signature=d739f0efd43a93514521f1665c8320bfec2e812939d2326c423ecc5806fe7be3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

