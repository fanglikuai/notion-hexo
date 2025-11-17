---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6EB4WIL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4%2FrzRlrV2QXhFQaAnGuLskVwU4ou12XneYHsf0H%2Bu9gIhAJdp7rWT3a6uKoe5H5zfdG5wn2p5Qu3VrQm56lhiJd0MKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMkwBT%2BITA%2B1yESFoq3APk0SG4%2FxIlhKHALdATjEXR0RHPYilS6rilQF2uYRk%2B0ZWWMhB8AS1KKhPw7EyRNXqPvhXZy0YWpRx7IvbBsyOKVnnuXztea51y%2B3gBFvGaSbwnDa2WemCHlkm2GcQUoWaBa7eTOESLOQNogvIFPA6hAZZbQmC%2BvBhJGGyTkTng2PFKATuzA6C4rnwyeOIajmEsgRoNlWNL4uCkD4NuVgVHcV8GWchXWUR%2Bc82SyiDsfTtRsyGdsU5G5JLdUhNTk83ftBCgwGNqbt0EY0cgPRfd%2BlrnjmTMDw0sXG9mtSf6CrhnRmfr5yAOYN%2BdNa3w4xhIkxXpjvRrh%2BHtsEa%2FKwfY66rNlDXy6J2IIMN%2BLjP%2B%2B%2BJ%2FYR%2BYZrlpUKhvbiM7AJrEvxlVuFw3JE%2FQkDC0NgMu9AfNxjIkx%2B8E7XyA4dkubSQZO5UHF3BIj2zXdX5THDUnH1ZWctP%2FU57BjP1%2FEXeToBYhz4bgCJiM1yVwXStg4ac%2BP3MsS3gt1F7eRbjKt9MfYmY02jU%2BOjvCZxg8fFmAkfOiX%2BrYj8qhEp7MwhXogHSnlHY1mbG5CfM41KrKiWNv28QPeyQhd3N4easRV5kIWhNZZlDc8GWWWf5hRFqfzDlpzE699Xj1YeiIpjDbj%2BzIBjqkAXDYLmGHqTEbkr7vi4MBJxZhi3EHd1qaMPWRMULQFzUOJrDjDi9pGszzRNJxZUHijV8V4%2F5h%2FTp8bsULbtTCijwYW98tyObz5fmS6M3l8yVac23hao3QeHIkXDabM4MtjoNQok5QmRDSjMdnG0wjzr4sDcyx%2FufiWneIWTHP1Q2ap6GVUf1LYj8huVHQVcay0a4ZcmA9XlyTT6auS2pLOK1e6b87&X-Amz-Signature=2b30d89b45bf3600a971fdc0284039a670fa9747a2760b0c587b86d88528f68a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

