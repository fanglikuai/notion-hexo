---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XNMCOI%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQCt6%2BBA76M%2FymIKu0jH0LTSkai6m33vieNuu9MUMI1mQAIhAPainWBSg3hX9ompSCdWKRKsnFvRd%2BFARBzhnSJsglOHKogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzGDUi%2BQhMN0JbPFV4q3ANrjyq95t2cnbX64OoD21GKAwkgayLJK2IVuoDDor3%2FuwISwmaSM7LtLWUoyb3kKFeJzH3CIRNOBi6EsKy3jU5A9Nn3cqhX1BQwbuRHmuQqws7PbQFUYH%2BmcqOfYj4Q9w54AWoFh%2BQH88I8FpMmMvbSfng8tdnlRMHpjx4nQe6Rm55fpigVki3xvra%2FalqpJbHFBghhWqa4qVKYSPIFUjWDyoqpptfVJcbA2Gt2ws6vACrvpE3umXWZikw%2BoGSlDMy2sA6UFXXGFmCsfN8blNdfGVG1DWsKCFWp2FSIQ6X9ileM%2B6DL5iUoUGxKEX%2B9%2FSTGtlQXkV2Qa9YUnVMgiDYYoFVapfZYN%2Bwb9gUdFXCeT%2BPIRfHprEV3%2Bwg6lyYFjYTSkRmqNKo%2BL99siOO8cSj4oVT%2BJ6u7UsXyJTUmqGPxpSm0MODrf75lM0gNuca4sHCfM5EKzCGErneqQ6KWDycdm9v1tHwHfSUw%2FAdcGqjAw%2Fg1eRIlhRi3g83TnvxhOhDTs5Gp49rWCiuC5i0hlY3Y8mfQC%2Bq3NGp5%2FMLc%2FSJ36ckdBL0UtcPJa3QYs3kmYRSq2sCUsUxYBY6qPwo2GpCLl1Q1KLoZ%2B5yBeFzgt9%2B%2FycPZazFO3m5kfzR9fzCg99XHBjqkAYHBwEDDRRVl3BujkkGA2VzWSWv6eoHxRtz1dz%2FPsOmhcXvU3RKJbZaErfqjO%2F%2BCECAliuMsSxvD3eSfPkP15%2FwZBSVQRJVDAbGTOq%2Bfqi3CWEkDePV4TuX8XPbtPn3iy4uKxktuomXTClD9Gtsw6wHPtKI4VANHvc3Qm0BqP0iwf%2BZ%2BHILR6ZInKZeDl89fqDE5rdYXNAcCOlUkz5x3lnLbXHqM&X-Amz-Signature=9b9d6f81a8ffb511afa21913addd02609a49944d357cdf2482875ae361195320&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

