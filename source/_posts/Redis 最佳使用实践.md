---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCVEHHJ3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVupvX47xv7A4x9zkHTDi%2FRkJgc9bI2nV2IgnIVUVp%2BAiEArWXRJLkRNz3lTgQOQpaTHHNAW3J%2B4w%2BQfixqfKXnzNMq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDMRF2%2Bxp7O4yljYgiyrcAzFHo1u9hc1MDmf9B8TBHxiHmAX0oTF2aGfSRHf8R7oZPub0swVliuv5ioCdAhrP%2Bz8cl7OiyT2hY%2B%2FRs3Fx1AMpbNhEl8SMoLIISBAAhLek4gfnRhzDi3vgHdj3scFRLKG8w7fXBfUpQKoJG4V2nUxRVvBFWFkfawiLVuqKHsRN3YHLdmXTN9vbxIB93TQHiJYit4tCtuw8q0mPVcFM35cPC7sUIiJRkPTTCoWa7s3NrLHZb8OpgGH4jom2FFt0NJFBwAdsFNj8yzF1lA2APpGEsd7tY%2FoxD93mTzMEHdUhNHC87ulwABDRaP8uE4pGqRO8rrOfxPB3dKeiocN3pTZxiJC6yoGx31KJ9%2FNtQc%2BUpEHOCwEGy%2BlhdE459sOWQPHpOdqZ7tI6hFP%2FJqGTQ%2FAyGXxDy7%2Fr6dySOaNPzU7Y7ecd%2FPxuBtIqxpZCYUyHzAYO8blxwtcSIAtnNa5PIX6lka1uLzPOq5532om5YiCkphKwKDBNV%2FMJw6xUIMInGMVW4YULHjpvHkbbuuS8mMXgPrkq4cmQK6km0mQ7ash6iH%2F6%2BQUdzwA4mtNLTXGdBkfuVpFgV78THKgVPmuJYAcLIi7UlD2j2sISnBJX%2FktGeeRx11PAlu1IsSrBMMeH08YGOqUB941VmpyoolnihT2SWFnWxNzOLy4yUfHd2o%2BnrjFRl%2F6QJ9YAm6EGDClb5tXOIKkxhlQiN5sM7r8uIqBnidaKWZXez5JbtR70FXGp%2Fh1Tyx9z4TGZlrzVt5viHADH6DWGY7jXxEcudegs50M3iRrEfisDriy%2BcXMCmnXpjqn6sGX4Rsa3X3EOwwUCuZdj4Gm6rmlKFeHKOZg20aUro%2BMxpfRJAqdr&X-Amz-Signature=d943043c89810d31f1ebd3e371d810230d4459c5389a03f236613152fdc8bde0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

