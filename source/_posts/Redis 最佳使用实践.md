---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FJWHFPJ%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA1AUIeHK9oeLVpJFgKVHgeXbpGbqYOMmdhMg9y5FviFAiB%2FnUowcuue1ctK5DGDYpuuoukjy0QtzEPmXhKWRyVAeCr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMcV4dPrwz8tl%2F%2BGLgKtwDJZ5A0DVwwW4RycvCHAKxiIir%2Bhm9uTx6a6GPNnzaa7qXELmcf%2FulEiE8fqc8fFzmfokT%2F93xqZhqag77XyDm4B3iydxk7RZTrza%2FKaEy23tA2ZN5zqVAdlatOtxPFY%2FZzFck53OFebXRRdXlA3hg%2FOSsKhmGsjn81qejK2viunN1fFd2eMXHq6DOVnoH77DvsMmfOHLBLT%2BRsb6jjHFINSNw0Nu3RFIQTGf3l9r0b4ll6chhBFvW5y9Ai0imszoic%2FjWEHnxr3L2kIdudbHkphZI1HCwwQWWplzHAn7tZrn34r9MKCTt818%2B%2B8E8FHXuW0gERAGbZITr02TQcfc6yxZCPuQ4QVJflQeItWxjBpHEbRfWnhUHxnYERCefG9caR8g%2FmO7i0o7LmEm103CcsBTCKVok0wswPKNRSEbB7jDbhpeQ3O3mxE3n%2FLIYvC5XEv4YzOIpInBI5pNUElkEO8xcRPJXYXaE%2FK5%2FD2RD9eHwlW%2BQosgJVQ1jM5Of0DAQ3t9ZdvSh1jbIGQzwilqjE6w5pA07Y8nPh6beYolAh4rxBkp1mV1qHpwub3MH%2BDZaR3H8CttlkJ7S1YsYdKa6VeEbA0iRuGcUEA8wrZL%2BvKHKmvSQ6m0rISbZaysw9eCDxwY6pgEJPDEO6OE3aYtAwipyA7qTby0o8pPzDUajyDeeZaQy6%2FyIcURjbhc7xhgg34Xr8nV4yyVMsbNqg8MRVSZEdb96eQRQ30z%2FvFVfmKEAqYwg2f6CqSiwyWXfoD59bw%2BckGuMyTCE8tZjy8LVprCz3Sd1PaNUofPrcD2KDjwpEpCMLEHvW6a7nqFVyYsZPtlNxXsdSf7JM0L%2Fm5gKhmhuVKbINQ6i3qWX&X-Amz-Signature=bcf6b609076ecbd85ad112263da04e990111385fef0b26c56596ca8f26aaebe9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

