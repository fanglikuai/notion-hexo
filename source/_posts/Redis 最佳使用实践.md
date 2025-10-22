---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP2BOGOM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCr2hrq78unOVCMbKQXtdrf31E4z%2FF9lWoJZPp1%2B0rv6QIhALqOU%2Bzrfj7ZYU4Z6iYg%2FksDRWRHawZVuf3nC67w2qIkKv8DCCoQABoMNjM3NDIzMTgzODA1IgwoC4sOuaC2tulSHBAq3APnZmuCdgGu4a7UBWq4EODtHeYew4IG7qhjowOwU9QKQggKA%2F2Q2JT1f0rf2QPtDeayE8bW4mv%2FuFy%2FSyVxBfgQ3DL8qkO8mjXlNpuUqSAxVMl3ltbPOSDMQpprDEszfv4%2BCIctRa8FvPVD9wJqFi6yDJDunRQe223XBlUTm8R0ww1QDRI262b2Zg7CLknuz2O6Gk%2F4r7dw%2Br2%2FEx9zW3YyxOFvIMPvmcQB7KTfPn8C%2FTikqmfiORjtjfooHmAql9AiWWrvYi4v0d1Z9Oh1dwxh%2Bk7rX%2FXaXXnEJvuzUAT7Cqr1EfpiahBQMgp620Vplfu%2BmpVOGE0qcDkJOnGBPOMN3tCDSlAE48nPmRbpq1x2B1Xb%2BBRh50xhR7u4yYW3WQP%2FDT1Au0JwTD%2F6WO7KcmivE7Law%2BFj3SG%2FCHYxnn0WE%2B323JTSMbNzPX37giJYTIwB16yti22Ttr8SM%2BCe%2FcRB4DgiCo6ixgtQyAQwCg8aD3sG4sABab7NRCfQwlpR8Ipoi%2FiIvoz2YOc4eO%2FqVBnESCp%2FXZagIpUWj%2BTiIdIwB3eXkBoOxt1Gv%2BALu0WRbpnY5K1aZOaUXw5xM77x7585HSeTacxRZkDP%2F%2F5QXPF16d7Y5UXswRqYJ51L8jDauOLHBjqkATYLNiHqRyWoCO%2F%2F8VfwgwTiaNPCpVlNAIIwtXPx7WH0Yr92XEEpUrV9XdoKxO2G6C6B6dZ5Pk4FXD6a%2B5k2YjiCHBwvpc7k1F08zkBuAYRm74rwIGdMkmaYB84T%2BUiHHKdgb2GfMf5k4%2BgDUIWK7NOvM5q4GuRkfpmE10zaaWOekURY%2FBkc51IxGnRwN8hrd3bTn4JjMHAs07OZ9CHAKrJdyQAR&X-Amz-Signature=2f537c550cb3f64573b1bb3e9e1c00a892a094560dd2af40ad70aa67231bce46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

