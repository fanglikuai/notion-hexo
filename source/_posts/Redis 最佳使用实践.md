---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFQGH3Y6%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJIMEYCIQCkhnhxATd99Kz256e9FfQh8Djc3E6fUi4NIKpE%2B5JgFQIhAMef5be1jrKZb%2BMK%2FZDWh%2FxhX8F3tvNhjsWZRCaVemzCKv8DCAwQABoMNjM3NDIzMTgzODA1IgwiaCB9uDLmtVmhoyYq3APmob1sB6GEbyqY%2BqlaIKtYP2FpEF0%2FE5Q1xl2G4HjIqTOEi5BiudNYE8VERv5lNY4tuh6IpPctjijCeSm%2BPGeCVmadBOdSirciXhNXFQWPdOezrJSLNAc2IlttiCC06HBO5ajk9ggN4FbLJqD9BjS%2FvKdd12KEcw0wL93s2r6soXz6oJsRJV5rHAGQ%2BKrDRwJOna7DuKwu0ccwVVMtyG2u6rmUGC5sZYBQeeMGAL97A2MdU%2BM%2F0GCK2R77TobduYxkoy7ChNecAbu%2BB6OQBJAO%2FNuC4TQWJjz6K%2FSiElrr8ZNScK2T16pPccRqfY5WmPafwUMWZHEmLASXzJDfGl1BoWt218lk2PeeofJDcQ0ldbkWu2ve5uzjyWz%2B%2FQe%2BNoItVJYY0l6MbOAAzl%2FVHK8cEUEkG%2BuIfRRdJScNyTxKX0ukYoaaK8iY1eYApHabvw9bZXAxqNkPGpgM0quLjZYKx4urY3%2F%2F6436TgxGTGs7PQpMBjYgmos8vOyJ3T%2FhqVZQZPAillqiYjx7lDUyFuH2Hew6vPUyslIAphR%2F7eLz6HQ%2F1ikCjnBN%2Btw%2FjTyoDfM0s41CSE0wSQY8GpFi%2FdJ4gByoZm5o4TwaQtJ2deTcy39qrwg95ko4ePBIgzCqloHJBjqkAQ1tz9fACMvB3SPbqb8oEFk3nxNnM9q32MF1PsyE%2B21TZ9GcM%2Bh7AYxX1DLC7OmQ%2Fwl3LDlrGlaJ%2BNvPQjw29xASiPnh50qn4D%2Fhs3Ihq1Qdt0dH0l%2ByUys6GLJKsA4wodavwgiNrcmAVVi0y8Qc%2FlaaYIp0q5s8hv2i5ZTpCoYmTDknkRd7uRd9%2FYfpOH9M%2B5LZpXEI24pD40QUTuP7iNqXqI3t&X-Amz-Signature=2941dcb911054a6547bf160cfc7f1091eba535efc4c68981730ba8fc85505136&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

