---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWCZGCHI%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE9aO68e4KR635XSJQ7Bvf8ABJIuNFF2MWIePVjvteW7AiBv2NUZX9GkrcWbe38FmqgmMQbg0AWU8JgrZ3Kj2vXbtSr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMCTuGtOvBjSQS20lKKtwDCVLNGdB5pvFMYfDUqDbM%2FubD7urnCeh3%2Fpt%2Bj4z7DwBZS00lk1KMYjKt7mYWP1Bvr4gAoI4MPRqyikEKLEpMwlHeFBYVqaAHRgw8MvveC8rYMV05sem6QgaepZzoRPnTkjuTzoZggX%2BkKabIoQtm%2Byd3VTbNulZb1GtNOZKckeYUGiYy2kUUI6n%2F2Qdahdk%2FLXuIHrULbUOgfHGQLDkl7hCCTIuQGdFUAcNWS%2BRkUepHZrrG05GuzdGlQ2xPs0dEPjQKXfayx9Kc4vso37xCn%2FKEBb8a9qpIT0j6iwBYzEwGe0Kbk1PDbTcwGIXfvFjXgSgmFES3AOH%2BIIYuHQJ3oLMz97eAw8Qm1DKhUNbYIWWyPmbbD8XfNzmRTHem58%2Fx2chDwltknLdqWNj%2B3fja9oukbvGRzxJPCe0FeI4YO%2F1LiCvRzXtjrudcrOctRy03DpkVEmYfCh5ll59CwA0K4T2BsCHFBh%2B4DhlavPDirmQTwNQB%2Bx83VoP4cPlgICt52%2F1WE4wLQ3tj%2B22xz2gNhZkm7Y5qOGWRfYKyLFy9s9Dl2P3rtLBkNym8De%2BLZPfMah5sOvS%2FmUNqDzaZq4TcO7pM6EKlWSbJcgNW22LbLh8kPq%2FUdTJi1VZ7xygwytfyxwY6pgHBdvgObfqxEartDORaNVWZWSszTBY%2B8IpCE81b6zP90XWp5CUG2MzQvDe5PZ2iiR0U80f5HdkIVRcYY0Q1ASYZrNdRdf%2BA0DTOZKmWXn9CyjbZwcYDU3HYEAhsKQz%2BYUOdrqSsxywzQh5a%2FufmCjOQK4aSTY5eHq6XyM220gIbLvjlnkAjKPm7HnwzeTDr0LAelUmbXvH4hzn2DeZc83pH2tdXyPIW&X-Amz-Signature=8213986f1cc3f527ec6c12fa92c475dcf8d4f3282603e1e8a2e46b104efa4d96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

