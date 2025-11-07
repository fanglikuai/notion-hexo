---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XUQ3BX5X%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF0wehRpAn7OS1ipa1%2FOceWuOYZb7OWDm5anxdwHgW2xAiBycxTGcmpod8%2BfSu3150sK8pE8UnXFhlduOgfCyU77PiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyxKOjNlYDp2A4w0vKtwDN4l7m9BUSQ%2Bxp7qttcARBnFYqKprKaqcIxgxMJjGfIqsgAEdlz09iQsywZ8LcR1nwbTMJcA%2BvCy2%2Fq%2FXfIiRC32FWogsIakxPqS4YrQuDGxSayKPmwNAE7KwJMjWiH4mkB5BLgHKg4fhFjUVuqmP9FMp1g0BCChoG14cWZxh8CZHom%2BbKH0g3TkuLfMM7yUeZSkKo0%2Ff6Q3MxArirbZ7GaTedZLTADMlEUTz1xzUDAhpOZU6EAZfuVD4OjaWvWlIQqfXtZVhAKxY8pLQijFQvCgbhIrJiwR3ZRagHuQB8ZiV6ZthPcaKD3VDbeTbJsnyqf7NuFM6zsl4%2FC7%2FBVVfmwAJZBEPhye6rZ37DqApGOsVh2g24KwjoFFBvNg4K90ATsQAfJLzehc%2BxH7dfKUXykL5wxQBc9r716GMG%2FYl2qT2yaAKwYFb3gfI6RlnSGEz4v6uLDBo44gSMZV3rZwNZb2qmU508hzKx1a1pZMimcnaaJfkFbP3HdT96BSiQ5n0kJqifgIK7mP145PbvCQhlsy2MUdGna%2F6cIzk2tbNZ%2BYiUDNUuY487Af92nI0LevA64JvdEevyfyH39Of1H1Ejr%2Bz7YcyA5Op0L%2FHfmnPwzEEUbIhGvDfieZH5Zgw79y1yAY6pgH8efQK9pdPoVackfSIEq4uR4bErKd1JQxgHqkpOn6qmCwb2H8k6S%2Bdj0zmA8sHWLlrZ5xd%2F5Ka0X9iPUhV0jsGMH8%2F5We29AdMUsJz2Vbpz0zUThSvXJ%2F9rrApizg6tAYrPkdWYz7%2FQdJnIokm337DXqAVhVblKsrLLO6uHLbnFulq8SU03GR8twWXKVscF%2FLcX9ceCyK2oi8HtXFS5egc8FJ%2BmDZA&X-Amz-Signature=498b83c2f6b84648c870ce4115a4c8a32f2720583a3ebe1c76011444bc06f0c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

