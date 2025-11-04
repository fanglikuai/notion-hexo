---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVGT5AQD%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcZDoAkkDi8YifUHBxCNMYA%2BjkT8YBSdQtAzIOUVOXNgIgJmmWi%2BsYCsa3Mle5u0y1UyTGQIY5QNkKLT1h3imM1g8q%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDE2%2BFL%2FQXxSB5UB6gyrcAwqDObrFnePvKqHiY96b2MC865udqCl3ldDmyImdhEpcirGgEfuBIqi5QAxjqnROWkohiM4DvfjLoKlbm0%2F%2BkQp9D1LuDkQKi7H9ORiaeRDhL5U3xAtjCAHgjVWY9vxKnSJVXYey0FCDYo194kO95jlFN1hjCoF8%2F0J%2B3CENhAWTMjjJPw6gls9HhNTS5g9ndIxeORsdNqR6rwOfW9DRGvRuYmvE5264QJEhIsK%2BC%2B5ELLWPtV5U76JR5dqnaGFWhPf1jJsxXsXGqTJ6GP2mfkVIXyq2TBBjZy4jqKpsHNeA5nfVJ2RY5%2FvefqweXjHxhY%2FDvsikjOkm%2B5x7iNMDiTu3WyWBXqa05LEfqHKpTuN3RnDdABJU3FDV95M3kQ33u%2Bfcg1R4%2BUbmQpGg%2FeSAH3Ht1A%2F92nND%2FnniLMgdGpHY3B5ehnmqdyECWNrVEdTc0tnMZXtb%2BU6zQbF%2BkFaL14fVMvOGla4xnNghgEAJV%2FUF%2BtANMzKAWL6ZBedlnsYEgzxEx1nL4Tr19l%2FL1x55%2F2u0fzYwKxfsDANsCJ1P6nRQtvQNKPc9g2tnmM9XsaVv8eKaxXZaUC5g9eH%2BAr5L3EeEGVp3KBcFOxDued2oVSWuAMfz5ZjJkK2HUWswMJqiqcgGOqUB3C5nfJiyXTuWZI9ndUuK42vlKbiWQ%2Beh%2F9ls8yaH1pN3feyIYmgzOKwO%2BS2F%2FrSsD%2BakELk0CKnXS315Gm8b%2BFt9ZS7OT%2BHJlXCYJgtZTXXvbZ0r%2B5caIG6m740YKB9l3QK6ftKskqn2Btu%2FqgLrBSxqGtnfRPezy1p4zQpUzjIMvbFGmNeEByC378HzcqlIR1bNS6JArpBULzJ7UZTmobplkTgF&X-Amz-Signature=34a11191ece26f9931c9613b9e5dc64108d1f72c2c810b0a2c3d30da0cb58af3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

