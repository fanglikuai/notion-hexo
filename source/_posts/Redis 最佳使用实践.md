---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIJYBHAG%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWd8TAPGxKKDMzyjBImeTjCP0RiZnpqykdL9DoE6jg%2BQIhAPj7d77r246WOP%2FEKoT%2Brvidop1tzrwhmQKOb7pA%2FFtqKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz6DUIz5%2FNLW0wCDTEq3AOIoI%2FWWiyKXyfeeLcBoI4IueCFRs2%2FHvAAsYT3dnvUNRNGxee9kFZA18Ua49gtM4CTbYFItMFsHLCZM5eJNmuyeykLhI7BFHSCOqrfjDoAupZQTJ5XqBGvuwCx9h7U%2FaiMQAz1JuIqJglORlscIuA7IS8SlQznqCQS2LbvngmdPGB%2F5GvqueqZmvbDm%2BFpdPgd3VbtX1zQJ10A9aCYZK5SJ5WUNJjOnmvtYpSejbOpTL2BAlinBIr%2FZO7h%2Bw%2B1nvd5k6YIue8GN0V4S3i8tHdobm8jirgG8%2BmQxuHbcYK7j0r65NyFWqhAj7Lj0EyPt1eqEXhf3qZEpCrjfVvZgioFVlWUtt3%2BqD%2F1U84xbXi%2FsWrdBqe2UQwU9b2lR7V3GIxLl8HZLn8XlFiGgdsQiPQHTxl9zEtDwBN496WrrNX%2FrEp4J1nF3Yo8szyEflrgZy21AVEGNwKNM8sx449d%2BVPh1eWFqvqb3d2%2BLoBG0HqStkD3JOCEkC%2BnbulptmYeY6QNgAhYp862IZbQ09nZKbmoaBum6SJUr0CaA3nt5Bl6TG9vSyqSj9oIyXHqpPLrz7F%2FfZZOTPnXTbmJSOXY1TjukcHuC92gWYbKjnJgPiwBpwe2uIIm%2FCwBNpjbnDC7u6TJBjqkAQeCQCG4vGRcDc45J2vuUhNws9jFX4V9lYiEZ7xpHzSA%2F23X8vOM52yruFbeyrdN9GafQ2gLoE6Pf%2BHnjVylB8LPYrsZwgeyrt3HGXH3QRKPJIPXH5YE%2BsQHkw3XyjKXWRRnce%2BLYmaYHiQD%2BGDib5CJdGTA9Q0rcnbqBWT3M1SqRcuW6hL4uwrY4xeHiDSjTfAmChWFRBGVXWlwJkunji4mf%2BLU&X-Amz-Signature=29a24944055841e23a18137446060c5ace022ade522312956ae2b7e27c7e9edc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

