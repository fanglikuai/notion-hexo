---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XZVDR74%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQD9paO6cYG2y062vF1QpG%2BUnN7Tg%2B2Xx3X1IyBjyIOhAQIhAMQEa67uV0QCG7jQGQ%2BOmqazfDjIH9VWZ7zCSBDy5v7oKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxtvN6wqZm5XDm037sq3ANNsQHg3c%2FDaMcGroTMdj4LGegwDBi5w5rFP1pHXtdQVAyFXl6hCUrCwTLsIjDc9Ou62dHbznLj%2BYcop%2FvDZMZNtZfxrRWEDY%2Bi7HNlrZ1b8xdlS%2FxEGIfxZjpqjUUwji9OkIy3TCGfHNvVh0%2FsTWEgm6bxEvUE5Rf4YXv%2Bxwurd0c0Z6OaA%2F1ukCeKBCkc6eDYuHhwVQqLGaMlwEOXjtZQ618UZW4qzhQxphLXaIbtlV8tqrq9uo97QIoszs9PsNLoDrpEkMTjHH6ExBiBqVEZah5wnokax5iSItDLodpiDwHi4JII0IGOvz%2BWvaQf60ktcGRT6%2FwOLFSWsokGojYFjSS4b4movdqc3pB921lY8e1uRiV3AitENP0d6ptwE4j3i944DNnHhRdJK7tL3W2bIbOySAF7WljqGXrVf9ZLjALeDZpuwhjAHwHJ5ON8vqXwMbZmShceiGc9Nhw3UehjsWfuck6KVCCkuZ2sdFD6AfpLX98K%2BH86J0jbJ533wuJZbMkcsBS1oG7tUP1jfrqCFz%2Fqnu8XYzejZER3d7X1cFoK217IhRnuzn%2FqGDdcFEvvx%2Bv1aH%2BwvHQqnOeB7bDnbVqsJdWk5qgqWb53LObXltfDCf8InnlkjYsOrjDN78TIBjqkARqIOv869gbld1kCjWyslxMeoiClmsZJ6L4azjpiumVo2LX7ExOxbIQPzGq%2FjewBOHI0QpRWrTZgnG9ZrQ0IQvKIOmOchRyLHnZHRs05tEJt6IJEfJfxUWvJd7R1nciHmQ2IKEP%2FznOXwn%2Fftml%2BFf%2BC1gxTbKkIMugOCHOkJYEg4HqAyUU%2BX6lrkNO6%2FiHVh5w6SgPRo00AmBn2PKQaqBLC69R9&X-Amz-Signature=d7e0c9ca78375770cf34a7df12716981c1aeb37150fff1c2a586ce7883ae4552&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

