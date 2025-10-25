---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FSQHBU2%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGJ1z9EWqxcGHLLGBqDhLUxLMXrMiwIpxqRfgLc%2BsvMxAiBjiVjQWzzRkMOS5C5ZzKqJOYtfzjDd8PWYs9qQxr1YQyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMNexi1HtTlhGuaPOFKtwDNJYwk8VrruTL4%2FJR9MFm%2FLBp4TrXhmJFQdo71TjyKw0P2AXx9Uvr%2BKkJo0JemqJtYV6QZHL4NzMMY9aP20vICThICCldvL3idPrINjrJsEQfw%2B1DhR7ZmruWyDjMD51ydIFXV%2Fzz9GPtgBsV6KYNcr%2BLWbRYUg3c4%2F4rGzLUUpZfGhgthfIqwiCU37%2F0yRBZS54ZKNEN9VLPpUrJX2oJpWyC1ouVU%2FeLgS70mH2uwO8A0dq1hVPQRK2bZYSb2E0Ab%2FqvK6P7CWMGS4QJgydtQUfV5Wr8Nxy9PN72WVphYppnjMnU0zS4Mh7n51vnmIJpJ5dqnqANZ2wZ49wTeSY9TMxWTMc%2F3yZ2QjqRLmaTnNVX1AqHnwF6YarMb3raaB4sk8z8TQqC3kNmFecxEIFOBUJkNEEb7fK4zGO6UrvlykFMP3LjUQpP8EcYqk82YdtGPNxwzdC0DWeUgBknODtNzj%2FKqt2di18AzVzs8ZQ71dHTg24%2Fy2b1b32TMhA3cPklq6mK5pRMsh6gsIvjEPwJ%2FkT7szFXMs8FsViSMwdjYUqh6TXnWEhGxPWmrFUr%2BvPhMNtqwiEoSbUWIqi4J%2F%2BP%2BTRkqrhwHAPurYeJK1ITXM1gWUQqpA4mDMNCQi0wjf7zxwY6pgGMIJ3N%2Bt1m1%2FpkUmXpx3zeivrAa3BYgZuLBv7TvHKpZuTJoDI0CXwhNu6lGb9IIro0tAimRtbAPTKm7DeppnJw17uXfFAM9jQDCRJGljNm6vx7PlHTZMh2GrYb2OK5YUx23jm2WZ6c0CvH%2BQShxMDchO%2Fg%2FYghoq96%2F%2B%2Fb3LZ%2F%2FWYDcE5SEr3XNkK81BQHARQR0ksDyHLwkzbEJDyTiS867tB2Bv1%2B&X-Amz-Signature=c6ec0dedfb2d7cc496dfe60e257959eed04e1afeb79b01cc268f5cf1bc3201af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

