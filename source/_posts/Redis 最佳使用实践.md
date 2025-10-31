---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLFYAIAI%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJIMEYCIQD9MEye1%2BjInCnqzWpInA1z14rNneoQMRN5LrH3rWfyFgIhAIN2qFFQLLZ%2F3QYEkWR84O3u9D9MVKyzIfCkLj%2B7G8jJKv8DCBkQABoMNjM3NDIzMTgzODA1IgyG24G%2B%2FLy%2BKht4h70q3ANu48FpbJKTizUhNp9MeE1RE71ZcXwRIJWp0l%2FcNpIZousbbT%2FN9gTkIeBLN8pQKPCWzf0ZEsSd7vtOQkKz3ErGzfgAlmUBK6cMOfFSodiotW9a4poLHk5Jk5XmjD30iD93VB6S3OOJxI076ALjhFmNSUmBLXIoxWRK6PxtkaRCnPcYFz0VhYZUFZ4pYCNf%2BB%2Fsb4eaiXWjMhMT8b4MkBvQDDh9lofGZGlviEHDzZLKvfTeptvKmqpMxHJ%2BelseFLD%2BFMg0%2FqvQRUMuXEUvqsnIPw4994rMvLU6v6DKW7YSZbtATrGmJpNaJCf0DbrGYjmK%2BvlJnNN%2FVeUODGCUHn%2BnGRxXCvioIOi%2FOJYI22IPj6JNW8k4D76PuFAlc6iWzmvMgxjMC%2BZyT97Q3hR0kyCvBgr%2FJKRPwjJUKO6yHK%2B45mSUTYR0fsabgy43NZs8EEv4X2hY5il5%2Bd3baVpTGTnBt2fhRojwkSe0b9kMZcXK5ao2n5MlwSJWfAaV92ing3%2Bg5xIs0liUnppy1NyJATxoMgnUPyvYJr2QVL1UKd3%2FcbLs2537PxFVRQH0HG4gPx9cDekONLAxU3GrciqH7yVuAjm%2FvYqf7kW44nfsbPkY0liwOAXvpwTSabK%2F7zD%2BuZPIBjqkAZky6Z0q18w9%2FlvAAJyBnVs%2FrwkZt08td8d55oSAzgATR48HYPK7Hzk%2Fl1Cs5Y9VdUCyKvhCbLQBYD8N6JQYvAwhT01LvhLSZYBB1knH2uoEwiVN6ZWk%2BU9L0ph0hutvm7H9tQZVLx4eUizWMM2zEfhPraW8lqjZVxJEcJ5bNdhLfymgvm7X0NZznzzeihgh8o0Ze893vg0yFX%2BWPCxcNBefmE%2BZ&X-Amz-Signature=c1bf76421866da8038de02232e3d29ae84662e9fe86e00de1871ffc9de4ed1aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

