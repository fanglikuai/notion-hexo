---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAOSTC5Q%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQC0PoOTundYwI5qc0d%2B1zBcFH0ci4XpMnpmceV8%2Bbp0iAIgXpbZMYwa%2BLUD0903kOO3BvCZQz0bQZljD3XiqJHEs7MqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2Fj1duEUBTypg1LuircA9U6LlXPz%2B6E8A7edILlvXMwG2AcSoFbEKKWYRhubwhNyBFBx%2BndWTATRARZ0zHHv5lkm1OJbRK%2FhK9H9pgB5aB0EfjGYyG%2FOOdzq8TOK55gqpyusadFwlSz0EGtymA5rIuvEMKrL77Vr%2BhtG9RISvG6bm6DUuaNUBezS142SbVmUmKUyyRfozUeJ4ObyuRI%2FW%2BIw8eg%2Fgu1GzQKgC5yDYamwJstDzvfSAP8ji9DrFjw5mJU6T1lYCvqpjWCYjPNCWD80icq7KpAm7OI%2BtbzM6RJstAN%2FHz8Ua6RB%2F4FeKPRvPjo0ErxsJweFAFvFDTOwHHJM3n7jE8EylE41VnRwZLaNSrfUClyp%2B%2B%2BZIZ0JDlyUyMCTWqr%2F6oVtnhmiPfblfIfsXNDjBszgTsCoa83xnWe9MEzeufQbPdmtsErq8RNr3dz4Nf58aoDCHT0u6d1e1pEtmWofGcLAt3tRt80dyRY1Y5Rh3kQBqp%2FN%2BaxKLDvbpfO0KNfQXZYIqZEF64aY59OdrYnHUh7oMIZxijc5CiIjKcYFaHRwnIt4HmzdRBpDcMrlUtW0ZtRXFzkvSkRs412UPlGcIU0O9KAMQxmXIjWpz9ZBR4zxJJ69nnxfZbMHF0m8aTszR4P%2FEAlMJGApscGOqUBgyV4UTdAPDys8NIedTDaeOzGeCMUVnPzjQ5HRZQvnTTeaHn9IqXWRHxZE7ICkOo4CWiQOgjt9Afp7nVxw3dgPUPPXtU8B8qMdyuTFjToBXFSKLcpDA2oiV6zORoTYpgzt65WLdcXCjo%2FJgUAQtdo1KjTCf0080TXAxfDDxVkBg2uIVdemoQcMuIxpprnXqZfvDyB08c5%2BOpLoFRU0b9j1VuwNE8m&X-Amz-Signature=a13465243ce418acc2163333711f5e3e47aa321af494c6256e80ac2f0ba4ec81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

