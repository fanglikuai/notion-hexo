---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DRCXFZN%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDJpH6YcO4FjN6NF7roX7lr5qun4b26VwOG7zsHaZFh1gIhAJn6%2FJFCVcZ1IiWOOm1qA1lt8MV9Tepr7K9wtF1%2BCNKuKv8DCHgQABoMNjM3NDIzMTgzODA1IgypieNxNHSlDbiVb8kq3ANU%2FG8DVc4QkpIJubD7DiQA2fqRzi%2FgWRnMgj%2BgAkGfXlDGELvGdrONh5FPhU3iUAb1p1963ZA8QW0HYX5rAXRMEjKtlEcAC6p9iVaxtUQ%2BfZI8SDzPSLwHpHiwt7SYs3bU6ESMe%2FKKuGMFN%2B3JhJIOqzpymw14f%2BPN9rXzY7bAM339T6dCkgx8n8LEhb%2BQPQTxnD0W1ZNI5Tk1oPGreTnyaeJXUr4a0FHl3MvE%2FE4j4rCAN1g6%2B%2BE4cwYLo8x1Yv7TeapY5XjCiCUsRj%2FjkZQusMB0fTMSz629ZYApL8r6hg7JyXDCzD%2BGrr2S3G8GhzceFSX9lYPsXnui1JoO79toZ%2BWmo%2Bl5YEigVqSSHCQoK2K4zTJIqtIzrwqGh3hKORsj9DyY%2B3xc%2B%2B2EUpOLE6351oBrd2BmHrGqHml4RMJCFOpTxamSN5H%2BksNuyE9JLfs%2BDpRnEM9qR%2F3G%2BNTBYO5IxSFfCAZaeyqrramwsXyc6EbGDibcWIeKRiZwet8CE3ZPz0mqhOH%2FZhEhzEW1vpCQHmjK3AlsPDDqwRlg4NTmvxiTsZjch7X%2Bmo5J3VM8Z7agKqjHzNVivt3Fm6Xv%2FNebH7Ncqf9SEALcy2EH0Z2igCgqfZEhh2d1TA%2FavTCA55jJBjqkAQUox9ditAF7muuqvQsN7LgofX%2BUMu9YGtPQHQuEubsfF45JH9pyaomCfTQXqGHxixNzT%2FQlTgRdDSO91Mh0qMUDZwuwVbirpaeR%2BfGKIXmLTSSPxmWaFTb6k93zeAL3dGLXEZidFL7ef01St4lyivKU4ZDLyfXGCUOveiLNDK2HVIafT2O0Mz41ZvORVQ1PQPoz%2BQ0VacSVRZuyFEtAiKO4qOGY&X-Amz-Signature=9cd26c5bce648400b62ea8bdb1a2a29f829aed11001ba8ec3aa5d0d76869c693&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

