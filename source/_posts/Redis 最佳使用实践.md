---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ECY2HUW%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ2OQaU8izDq9rvbLZP7c4JNOG7eYRgLDO28VgYtz0kAIgTLE17EyIbAJzNEEIf7y%2FznANpGoL8PzKKZGdEy1sSmIqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ6UoxUsqvvoV5b7XyrcA4%2FypfAkjfE2WBnvi%2FQKlzBCHVLmESGi2pbJEvsiBEGZR%2FqWMVx6nOoD7zbL6MDm7VGCztkSQVjvkWPhUnIrgDbwrS8NtJFJz2TyJk%2B%2BWW6S%2Fu1WA%2FOGPgeVbfxUUErS6YeDQUR6Ai5TNtcvPljH6JVnJ8iSqVnX%2FUQ7nRQfW2vv32uHDKIHo%2BewKBPfKwSMaT1ZnV7Wip7XGhTGJV2WsZoPR2lfiQb9BZLQw2S2b69PrS3%2FdWRggeCwjMHKCOeMCOVAa97zIKG0SJ2QFpmrhXFGPyWGqukCWLp9qAUTo1NFElD9OeMITSMZ2cl4VzYlGVhsRNEqcDqM%2B9MzXA0yt6ITeFoj%2Fx5XoNOPqCOb5%2B7qUzunEN97UjHaSq5YOJBSpOOAKNT%2FtdVwvR8Kv67Uq7h3PXVlQbyRi8ie%2F4CK2YOkbA4Dtk6RDj6u0WolZZhLlvurEqlPpxbAwxnhK9wlFTSQkfvo4%2BN94VhoowuLPs%2BMjc0Wq5J6Y%2BeWOpNDWpHGIMNCpuNr%2BlCbge%2FIGyje0WMRs70VcQ7GW9rEEETgM8Tusm7vNx7%2BEIq9%2FUCbIKqONMlQq4Glza2c2hEgvhRNJq8NynHEmy69UnNPGgQkADKdDhRilJc0otnchTXBMLC62MYGOqUBSOKGxsPu9kDDAibp7lw8JEmdo%2FnnusfTl%2BIlxdoEJWK7i%2B0KrpxqPNLhELLFYalA3gBXjqtCe6rmMuS%2BA5LRHxoYS9mhXIGqWoImQR9WIJ6REpFysiSJy82pwSzkMIgezK1wk%2BST7%2FtmUr0B3UEP6rQv%2BuIFBhVf7%2FAvFycHHYrlLH1MrtCuC9vumzjDCOAut%2BuQ7BZ4aSQJ8Ottwu%2BVpGDDsKVp&X-Amz-Signature=cc722a535b48250989f62a6eda9fa9459cdd31e5c12e8083d2f8afc23de0e9d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

