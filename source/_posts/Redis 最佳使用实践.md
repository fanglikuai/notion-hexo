---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UEY5OUK%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCICzWNH7bP17WM08a232DXsBTjzGSXClhZm9PCoM1mt39AiBmKKdB%2FnweXp%2Fpy8KToBmIhUEzy%2BkNWUHNmcldHkDFWCr%2FAwgSEAAaDDYzNzQyMzE4MzgwNSIMK05UB%2F5Vb540EZg2KtwD4w0iXHVeMjYSA5k9FeyA%2Fm%2Bl9sfgrCzeSw8PRsYl9%2FYG6MxQB2SR7WsMpXX4YZASUVEOOn0R%2BQiWRKZXb59dxNndVZ%2Fr14aOgGZdeRzFZ7CX49ZtaEfNJLJ9lkuHXVJWhyIfey9aCXA2SBOLE6bN2tvDVjhvzJMUmJ1QZUK93crCPCqIM%2B6Uy%2Fd7wfctoQnE9bPqd7CuCGoFsSabUqZkJ9zCx%2BmICE5o56Djv%2F1IOWBB4EyOfyspUVxnKZUg3bX2Gdyn3o0hF0JpRpi1hcS2wi8sLBMeWQoPBqj4XSGtEyx4JdlsndKquZdesolK6mYNZKsKs85Yv5GUuxEen7bNvDPsr0KGm5U3LpLHFOhE%2BXbENHLU72igfEm3U1IIwCl6tmNzdVOHQexXg6arcJSFe3ckIb0JjC8bzSTVX5pgPiCa%2BxFE%2BwvijHzjTBoujdNNN2lzp0RbTTp9Tb3afRiYXztzA8OUYHHiBCa51Opjgz3cs4QYKDQK8Vu8nVsuffsrSvsnNeAoVzqF6cXj6gwGSpMAa14e76jh9gv9OxFPKd3Wr75RvdPJ76zjlC1afAwgMH6fqNKrgFfAF%2BKzSwjZwOJfawgaKukTJbPzuG6klTUhpiESkyl2yIkmK54wyeqRyAY6pgHiQaH3ruyzj0riYC2yM6VsLhbceXsKxmBA88S%2F6WY2B5K6Ux5Faer9tsY1QRN2vIDEUrBlGwLd%2FjjR%2Fwn%2BqtlA3%2BSQy%2FOcHSpHRqaUD%2FZ%2B97ggV1Z1Tt5QUGXA%2BZrVPBbBcACYyh0PUa6pNdfe0Gg2Zfmg4w2ZHatGjt0V8O8lfvp7GinQeMU89N%2BAi1jSD6fF253pMhitCo09Q402i83D%2F9W%2Bo255&X-Amz-Signature=bedb1e12d4884d2e51f85d6a83dc3eb69b2751ec5224243a4681d39346c063aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

