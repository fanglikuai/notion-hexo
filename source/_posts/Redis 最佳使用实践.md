---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJYYOSDO%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJGMEQCICemnaAOBfldpppxwRa%2BWUhpYzXNUhjaRC1Cche5QsIKAiAJGkVFJRi%2BTotLV%2BZO2vQwPo0fJqHG8ES%2BxbzepZmBcyr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMH6uEGbKFo6%2F7uQA5KtwD59G%2F4rYNBccfi4o6Uvm8aK7nUufO%2BR9bFNs%2F2pBPM02EaSujmMoQUY3%2BA5hS0dyH3uKMxVshcxIqg9zaDKEb5HTZb%2BoNbK5UIhKpA%2FEWSXOQPYakK2a0SXkoGOgdPyPGQf4B%2FgDsKnWQU2CFj1APu5cYKNAOEkj24fCGW%2FBkdzJAjgLZ%2FD36lqYmBA0oMMxO2iunl7rSzHtSOWJwTmN6k6OvXKcRQ8YWegdKuCjFXALpQSN821XdtYgBgo%2BtXsgMbgtFoEOibFUfwT8n0EHoqG8TwPzXCheAjoC7k4l9E6DfwHHBlHuBJ8ntL8SvefIKfNltLfHtZB4FtvEc0Pb8z7m6z%2BApgvinjvhZ%2Fdof%2FZt3JkPZtEALainZYI8mbNoPrP6XR1m5JV4SfGTYg6GbX1cz769k93yxQ5JJlt30h%2FuiXRlDokXL2LEh6w%2FApbyzhvrARZrcieCV%2BRpj5D5fePF8LLMf%2FsK%2FCV62p%2BJZDsqiY66KHFGTNuSfB9D%2BvZovczACdgbQHmR3UP6XiYy8Ajy3UfkZN5xxyJj3QGDxOOGVYzarvX12UsPomc87oJUXRSsMSp3tXkxXkcGzo%2B43FBup3QQqZFgJ33ty3RVyTDCnFcpSau%2FZYW8fAyMw74GJyQY6pgHppQzmOxAden1PAwn7raoqChZZXCHHc1Un9%2FMl2G%2FDg04CNJU09x7445zpUo0%2FdcYEWAq3aqJ8aTb9e5XkpF1xHz81Gzr%2FeG4LCJ%2FxG1KnODmmQHLMa4OzMmYSFgzY93nHhEIMByE%2FupLk14IbQy89dz6kGGCAsRm3RV%2FfsgS8BnuQFbb8KoP0XsvwhBny1MbNyvju3%2BaIigOLvD%2Fhj6uxvkNnUwBu&X-Amz-Signature=dc7acadd269d43302e676a20320ff93cff67b09196d55bef41c0bd5eeb2c5623&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

