---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MEB2Q5H%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJIMEYCIQDFTJyJSN7wliUT2fUqNL7zYrV5IyNs5AzZ8hJjiNL1hwIhAMffrRdvitLmjQp869Mnmtxs6KyxnqijeafO000bS45nKv8DCBcQABoMNjM3NDIzMTgzODA1Igz4B%2B1%2BLnzJPYhqNKYq3ANmcBFAv%2FpGSjWS0M%2F5%2Bo8qPZrJOexq9MOtL0zD9G5MMDR9FopHscFgTyYogeH%2Bpo7Mq5P9CUe0P6K%2Fqf%2BCf8gxYREWVJqppo8LbV4I3hYwhDZKRqVg00KS7swyOkhHR34nmx2dOhjPP8tCRWEerxAQNsGkB%2BeP9%2FliMrSl8M3%2FdM8nVVH1K7M1kjor%2BRWP5qbU%2Fk0LhDwXNoeAZ7zOoTe%2Bni66PVLgbRFTXbp65u83YmZFemR7qFzxjg8saB%2BBMtizguYdfP%2FBIml2d2Du8lf7tPJSUAnowOXfj8Jlmdqge2uoUw01DcPXoU561ECCGEOVKoRo7IRUgfqh4pvDgDopDYQitgIfk8jTP2L8G6HyDrZzoQSEQhJUlp847lmMRfIzpUIDitTwohFHtg0IjgMgQnM9j%2BWqu1t78ZFq35No4kgRt9e9TUWtXgg7C96nXNdNIm6U2NMd0ztuxYKZpg4jfR48VHQsGo9Z77lgg1tqoAY6BRduRgzRM865xcxh7eK6s3zpdYquBLhTJkg1Rf6bQ1cMsmXHEQuUGExiQemkDezvgxvb4s9x0bV%2BDgDWbvnvC8UXb5kj0RzuzCyivTkDWFL6wamEZz8IGtjDzPxlZwRYtL%2FnnV%2FUYFdEVTDBpcvIBjqkAeN0be19ZCLK4POSMC1q7tBLK5oU191CF8EvyNcBmPgzkbhRSB6d%2BCJnrcuvh7cqtCgx2%2FmB71EYlCc1dHOxe%2BoACHUqA6GhhzzGpykuNtuneAXS2wIBhUOoDHKmn8pw5PZ1HTHtu3vNYjuTHGQ%2Bh7YYNsCqH6h0%2BOZFeMUm1EWfxYInVUDlgc1aGFCA5T%2FI4EoWu4A56OyXBWokpYAqza6P90%2F%2F&X-Amz-Signature=14f7bb61e9d87225298613775605e355c4e2a9c7cbc331e261fcb2eafe36786c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

