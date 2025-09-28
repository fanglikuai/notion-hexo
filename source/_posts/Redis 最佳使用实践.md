---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPNHRJLS%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIAq17mbXBT2%2FMh4FXf%2Bd480WBjuJoVjb0RB4bvTaAU37AiEAnxNlqfc0Udu8G4CmaWPVv8imgsKpc7vT4Rt%2BEb3SeUsqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAHk0%2FZWvGf%2BfVlyhSrcAwEpJZiZOW3yOYL3Qc3QhUHBjuqtmwJ6OeeJ%2BQV%2FQD6l0l4CDA4DW6l4OnmSJ3kg2Ri%2BTzKWgjP2otBvUI39qYlgfuqud2%2F3yqobMY1NPm4PolKhVRWp%2Fs9xNs%2FAIIukyg9jxvWaMr%2BMd%2F3OWPusdN1EQ9uA0LQ4GrnX9S80qpUC81GsEWCs2sI3m0mkOnrdMQaZFgTkv4m3GvuOWIiLr7x1wJscKHODKWAJXC0%2BbtMsrsCxDqQIcwQ3SMZl31CTr7LQYxl5JXvoX8gpbGEENdaJSv89arP43gIJyArbxdybDeutYeAMq8kAaX45uOAzLUeoXnlYrGQ4zF9FXTF5fyGHzQW%2BH0GAnQDeIwEMVb%2FZYgvlDNi6XF4KO8TagEoi1mew7qOlagm%2FL90IL0Mtz%2B%2BjX%2FIDGluZFARRGQSgZypz%2BpWlWIlt28BZGPyAH5cetxgQWBpU%2B%2FJP2ugWlw7LgQpAWXbFp5Ub9xvynupUapv9xGtTIG1yfQYr776K6IEV8NkXBG2z%2F8RjBSzqRuWF6iDpZs8a0s259Rd8Y%2B9fdbQgkD0uyQQHlxLyLVFTY5PIo6nVTmNB8b%2FU8mYkKn75UTvBEG9Xmh3BSoqXUoRM3fnSIpCd%2BHBeWy4wYJluMI%2B948YGOqUBTxlQtdoTv9yESLejWTgU39PxiJNtpVze2EFT5Yr6iRDhxwGCpe%2BPzI0ZA%2BRNC7noQ8san3rrW0lX3TtmQukt8vOT3fE3j8QGHBbmZMqmzxWiC%2Fx4a5vcjnwDaI7GjFeYQm%2Fh6C6B8%2BGDB9WMb9jdBq3AXJzt3i44Uuil09jUAqclfww5HNVwa6AnfYRl2I9hsTJCrS0cust1V8yGLjYabNn6uZGO&X-Amz-Signature=60a57f364bca621b57864b8160b48a9d131294467698094504166bad11ab7e8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

