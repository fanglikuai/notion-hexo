---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ONBHBTY%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSAwjw%2F%2F94sXx2ZCp56J4ybXqxb1T%2FO%2Fg%2FMztHvMwz5AIhALq0TuePwyiibJMgZFc4iTZnghuHKol3at2ZojvmosxxKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzer9%2FKxH1JllpQMuUq3AOjgPgpnLdNjB%2Fa873oTFSihV6ixxEJ2u80%2F9nOhWmniW%2Bc4wOf0Gghip2%2Bc19LjWxz%2FYiX5DTSchMKG7fUXrNCbiRxOyCnqwCRWsSJtD%2F%2Bar7IparAQigKZzAgyKWhQW4vF2IZ2nJyuChghUx%2FmigjQkjET9N1DAVIJSK5m9n8MPVaOVwrenSVdH7u9ErEidp1FFpkg7RUWqj3RU3izmwDqpg%2FgM7onRM5uMO9clFi8K5tWSY6BVRQtwMnaWj%2BfLlBwAybcwnvch3fcIVz%2FRUhbn8W7XVtWJOT4qfL%2BzI3%2FcMRcDbzAeXUR4hYwJ1g5WP1xW2mefQMeihl0WqlCEm3PzvDTCmAk6mkmqbjug2meQWnt6cMulGIe%2F5f6yIIRNWN56ns51DkNI91hDr6PnLsi%2FZgrfIK7yfx73TvPl1bolY9R2ShgppBqoqBLBtZwVG7Cf3tErOyPmR3Xjq5Qnda9ZdvTG3%2BhxXPgWTe5dta5iHc%2FDRdhUGEvXBf75mIimwT3DC1keOrPRDzSLxjzTcB5jB4wP6oyZqSTMAYNXjEsXgShXzGTf4N569Bem6tlZ3UTDtbb8245WDBknqNQOtydgRWnBj%2BPzNDcSnAcP461Uqc%2BOueXdtxFsbQxTCrmY%2FHBjqkAR8euD54zJaHlX2PGn2Ln%2B60fwrrtSNVUqwlHUGJQTcRmtz5Nhm0IGYWpJBd%2FHb4gapsdi8rh9iqVZD3yAUIslHlIB0ilgx69L2OZ8%2FX6WHLZHiUjJU30yULHIBWD1NmRefDmZaTf7KX5Ofp1TtThJrQtbobcfcvKrFgVEcymz1J29v3M6Je%2BNCAejppJomvsA%2Fr%2BmBE3K%2BzS79qqyVEkC4lnk44&X-Amz-Signature=b6e4720b1c0fa66fac1b7005de81fa85c6023ba22689692489cd5eb79d1c66e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

