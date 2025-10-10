---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVF5FZLX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T150119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJGMEQCIAFtl8Axq7vH1TLcP8WvBt3umu6tT4nM5YYO94LqZbBJAiBPY98jLuzPzO8pDvP4pGWsq2Jvh4Tct9%2Begrxk0MtcgSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCV7Z99yjNQTnZFxTKtwDKmk%2F6RsQGsEDGSaqd9zce%2FIAUo7UCiZoqGJ6jA5lPoYbxxUKxLhNqNGVob4sFvexf7kVZoZWNqmhY46v1W%2BCNL2bCtFRN6y3g%2FeBoa3lPK0y6niytcv6gHkxYnvm1tBfQs68yZNcrfpUbHJxDYffOH4qy4nNXjqQka0Go%2FLO7G80WHIPjNYw7Jqxkft6IoOiwvPYzPwyeMqZ3cfPbAXCc2CDmMRCPdEtVOjqOum57JS7dO5nk%2Fdhs9QYQUxj91cEmhsdDfUAv3ROqRzl6SlFj6EBSTyVBu5%2FgDW%2FrKQrMQ%2Bu1A8JATnWjrhHsbKKSI%2BdoERBcxh8sFxt5MTq2Ft4rz%2FIS19hJd6a31FSIE6LZDXjIg7xsuc18shjLF3MHkO6h0NR3U3rrqMHRlfHR4GgDigmUo4Xv6fLn1m1CS9QDXs4T8El%2FUFu523z690cmDOB8rt%2BNRzGsp9b7wvNTZ3ln0R9aXYJD4ard45uWf7yO5207tduF%2FO%2B3Mt%2Fpfh%2FQXbGQIACSoyx3lcdENvCJ%2F4z%2BJ3SFONHjghitqYbtoDI%2BkNMTGacTXsOCmmhxcSOJA0BHVMqo8%2FnNHq83GcpWHnsK%2FJe8GL0%2FDovQUY8yQMpRaaEywzLh2ZwQomh8GUwvaakxwY6pgHJwWhMFata47EpTpss3rV4rDgtjUxMtz7BpAr8X%2BLIw1k982AstW1CqLY6qwM3P3vrv3xv0H5B72Z%2BFtq8s3RFUwjhqHrTuCQGprvbiZ6%2FKbbvDJNN52YrNta6zelml9GeKOmFBq1g3TwVtBOHynTdyOsphJjVNyjn8%2FU2ve7RBGYIEEIkNwi7NvSUHtbWyEofasjvPfHpN%2FTA32wMwfZFaCup95Gg&X-Amz-Signature=f84db0a900f8428806eaf8fdda22aedccd50fbf9664e8363baf88ae61a3a40d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

