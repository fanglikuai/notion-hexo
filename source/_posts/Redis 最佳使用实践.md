---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS5JKU7D%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T080101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCT78bgqLO0YywnyagS1S7xZhQGQvg%2BR1eQcch8vAbnCAIhAI4cxxnXlBdYoz%2FHUknIJHh3ueXzAjsPYF3%2Fp3monT5dKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3P8w4CmLcWxBXbXAq3AP9gVMzZj8J0dGmWcgBzpXy4nDR%2FbrH1c0pVYDz3dP4cfesnJ8Ucd%2BaAmPMtMjx0fpwVYKfTp5gtYkwIgfxjI5qeIppS1aNid1yCKTFWsxTUBkQJRSS1FslF6gOGvbZb4z%2FSzgyeaeebQBfDoobqRzYedJT%2FzNGdOLYx1EhjxKk%2BfAsqQ4imrdjE8%2B8QYnvJY1CMfyX9aYIfPdVlq2OzJzPRARX%2B5XfjwGC5B2TofwYjUPC4vsb420wnF%2F%2Biwrnuq%2Bz25wSPXJj0NbvJPl4dR1zZKkjQiZ%2BzGWe8lp1zoL%2FTHZyfUBe3Fsqk%2FC5uUfd69EdXor4bwKq%2FnJkSBdFeFT8cvBPhGqlYSTgX66Cn1GxZMItzJW31vbQikQB1U5ytjgyGgzj1gAFpe%2B3PrCacCTh0iwjq7lqW88M20kosGteiKe2cFCI%2F%2FuMalKJJT4OuatvAAj3TIQfShcwy8I2SayI1VVzSQDml8AjJgGCYsAvdR%2F1pEh9uGznO9tCymC2JyEPOziWBBH4JF6yhQCT5%2Fp8LvgFTFr1kvc3gvlGPtitj9phwhRUWQ%2BPxfmpHGbqmaj1v0LhdjSTfFrAiGJTQmNIGBHpDrWXjdKCuP1I1Klnt19ePG5WKurEpQU3QjC%2B%2B7XIBjqkAbaGMg%2FYU11lgEkW7ADjmZgNTDX7k3WNVK9aY5vEwdpRJI%2By7DUX1gMgvpbacIQ0vkv7g3fdUb8yd2cci9hCm0O6ElkOPHC3AKOhFdV99dfBQnpZyzLKNhT%2FtGHZlHvQnqwrad1YT5Io%2FVztfdS1VjSkTKXRRuixC2ZHthsQrp91w%2BkUTt6OJGHyJXlGuMfZvVi2ew5msigFWUKI0hpHLoe3k3WF&X-Amz-Signature=1a124615ccfb6506eaaab497c5be5b9a2a76a9af1279bf8a69a00ecf75785384&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

