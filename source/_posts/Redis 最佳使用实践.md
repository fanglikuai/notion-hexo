---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4YTHDUX%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD8nGYQQnzMNYYZUY3QoU4yd6vtZvw0vvPdc9XDn3ORUwIhAP2uDId%2Fu5OsIEszWVwE1kpWZx3S6doYLHCHcjvfgS3%2BKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyphl9lkA1s0q%2FX0kwq3APoIBb%2BqnIvxI8Ohb1mRpOO6pC5SLs7limcQirt4SxJr2DE1kEDDhFt2hx4tcd7nFPQHlsjHnM2ahT6ChSlNoCzLUcq9tQHV6UrT5O%2F67AV0ST7gSWQVM871rEVqnMKywUJwFbBogdvvPWZ1550lR4YdBzhQ%2BlIuED3WHEWxiQL75AhBWrYPoymSeqFCeS1O8ch1j8ZE4vZ07VrHEBpEeUwM%2B0Vw%2FHOF1kS6mrFvc56OR5p8tmMRqCWP5HjrECqhoaBW2xFHgL6wnshT5WBUrhjfRij8vDdF7J4oKLMMcl9k%2Brun3pD7b8yQrGpURzwxCll3d5hWFOqQqwOc0z%2B6mhsZ1j%2BkXLMPVcIj%2BAJyXIjIztiYSlq1Uk%2BDJcIMQ%2FdY5y5ae9EduTr4JQ%2BodwHJwpK43sL5HwYbrhgW%2BGBivy28aJrv%2BVUDgv%2Bna4jadGfVSxWW7jZZrAzbMRshE%2BCa8sSPHq5hoPq7vzEKFxbUyAkQwkKFiI2g8irubNyMlFq6Hy%2BqBYWUyhYS71AsLbkb9hwnH1fwC2g1mXX85znvtDpSMy5V9mKgMULshZEK1RP4TgmYyWq5aOcMQhtWf8YQz8pFOG1Jqbzp0Xxx5MePaYVovtfyO4vmkA8%2BWbymDDuxOPIBjqkAZdOJKbRhWyzHQWGRQPHoboowzYG4QbLQn0Bhypj8N%2FXMQ4KAEQkUZjvqh%2FtCRMT0gW5fF58RIzlhP88oiGrAz8gpJr7IQF1HWrLCuqeBrSb47CW%2F742nq2KT%2FUDr8Thjh7Hf8PbHau28rP8lwcuzNFWCgZBeF5KVGUZaJh%2FER3yuFZ1cyVMikfh%2FyRr%2ByC4yapA6%2F6MW6tiBNrdAeYqitlxIqbD&X-Amz-Signature=3d1d27379fcfcede97477b04f8013d64b522a5e11783393aa2443d9efc2762d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

