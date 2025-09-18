---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZF2HVST6%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQCzX2OsExSMtIQE0tSxHsTb7vFOfrERNpH73al6I2WTlwIhALL7iPLSirKOuR7w63NIX7ooX%2FvvtqFTcC7XLxcV2QE%2FKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzrUrdRHoZsPDpRqfwq3AM90W0lO6jhy7NAwLb3Qfo6Ho1U3J89JNpnKzxl6hrmMqYmm3AvVrGKKDerhy9GbA2JFZusE11fDyd4pAuxKTol80S3wgpYfFRHDUPHS0aT4jtRw55PnKsIuiHvZvRk5VE7igsyjE1prNp790nSFh2C8vll%2B8N%2F3%2BFZ%2BheCXQyAJh6wWfIntMMwMNX3cNSwubxOsxS%2BCOSypOoFbM%2BdKXGhYZdLjUglXakffLefYuCXBlV0ggAtSoWVIIcYqA%2FP4Cz%2Fd3oO0BsWJ9EjY9YFKk1TPGvx6voMgLInOVSB%2FnmTtf%2ButiBRMCyUG3%2BtMUWhtPpoA1xqiq2thmwkq0AwBJK9U2osdSUryIyXA7K%2F5RVYYNXnIQvVSpmQ8281WbR%2Fs6hBOyxhTWbgQhYlP0c491Rf7eGYnlfzKFl92pJ6lDcST3Zwc9ql6d7OYPB75f%2BKazxOzuIe6paWbMXVIE3loD2h5x4WIePD6V96Swch7ir71mTmLHXEL1DBlzb6SbrX9QoTlOkefq2UqdMbfA%2Bi1sdWLcoVuH9ElkZHMoLXdAOlm6t7ChO3LM4k7Od4izWtQZnkLVVLFxqeBOhdQ36mJDPnSsggeyQ5yHy80%2BOOggdIqbSV%2FSh4Pn2PyhvgdTC9%2BbDGBjqkAT74r%2Fzp8TS2mqLzowBALjmIcNhuLgWlVV2lPlQW06hs2Lff%2BNgNstDnNkDPOcH8L%2BEWTTtVEdNvDFXDWQEaiUOpucde7DS2r9HQvY2zW9BWzE4Jd%2BSWctOUGjKEsZ0afgHNV9u2Fws5ILPp%2FJSQnIYmbgjiWnPiVSE2nVNSmhilmUHPJJndDK96OWlIRB%2BV9PKeZJcvoAuGwHODknAS6N70nAIQ&X-Amz-Signature=e27bd0db0609389cffba32fcd0c99263beba6ccd0cfbb4cd2ef8fec6e4b93681&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

