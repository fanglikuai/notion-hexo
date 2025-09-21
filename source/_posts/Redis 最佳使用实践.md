---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TTLF4CJO%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBQqfDnFKkxbaAAEa%2F9gqn8qNn2zSc785WoCNayzGLJKAiB2iDnL%2F88y7aOSSEZL6DKrx65j%2Bvgt8fAHvWBw5i0pxir%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMp1la1xE4HOuETwNhKtwDprSmcqPNsxhRjyT%2BhCNjc39ZGZAlayYsSOYEuJTqPuFR1pZbqa8tnovzH5IPzUsWKN%2BhkUQFJFunh62nQrT0jUH%2FN%2FyET1dmVscAfEqhJ9VGyrnoR%2FH0GQvqR%2B3DD%2FGzO0oBJI6ZwxiDQtXMTwqpFNcNLw0sXIRvM5i1zkaId%2FOwVxMdtQQi5Um0mkVTnfIt62qMuKZZUMEg%2FGopjfax9cBRo%2FDj%2FTqGzvTQrr2tW5WSjKuKsp1J67TQWOdSYqYYGFEcuqK2pImq30%2BRJV0IRnDsTPW%2FPKnydCTuUJBCXDdU7gSFrLoSJ%2BRfJeieMI8CEvkYfox227uGdyXmyZ8TdWGOMeMsHdMFBOKNIQeTrcdI%2FtR0vZXP03Xn7aWTDAj%2B0SqhIe%2F6ibsVQrttqnDviJ8mhznWwKQXkKs%2FT8QXhDzHu11fV3utjBe0fIxGOFAWy6IJlPwp%2F6crDngIUdFj1dNVle6FROaTD3T%2BGGUXvxHdYMnE78OgZxSFvPb9acj6K7wtiu8ElH0ax7%2FtXTjMGH2XJhpt8Lyt1JG3kLd6b%2BPLgPoRUMBmnKchSqPAtlu8QCCGO1FMN%2Bt0LqupMHVt4GxveavdfazERm%2FzU2JlhSd6ZG%2BIWZAjcfsYSsgwvOjAxgY6pgFLiQA%2FXl%2BWZrwASni8ppc2I2JXDau9iD3ejiAViZPcl8PVazl%2FexAqXDh%2ByRCD3h1MsSx0Kn4NT90VDBXH8%2BV5zXD5rCkPF45b8J80OEN%2B1%2FzjMPE9ZnJfuKQbZPA7CoyxnuFMVaJVEB00tt%2BlEQw%2BMweX%2BBfuilbZVWtMezshbMMo29OzNO9jAjXKSzdPnxQGz5NVmQ%2BaT44CbDMUigZJ3HFRE2X7&X-Amz-Signature=a54a207015e9848aafece45340a6fc62a7b5ff659ef17185c588d1b4dc1b8379&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

