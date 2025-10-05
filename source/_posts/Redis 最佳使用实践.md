---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVTZ4AE7%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T170128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0EMtZ01w0jAfZLUVUZBQQP12JwbcvArN9oHHhVWWSjQIhAMjxg44UskBQTX9nW%2BtXY6Pa1CmInWLZjIZohzBV%2FaRhKv8DCHQQABoMNjM3NDIzMTgzODA1Igx4mBP9whf4%2B0SwmV8q3AOPbgX30BnQVpUOLVS%2BDnr1YPRXKizRJqGcMLv%2BzyQzhNcrLzSR3VYWO1LwvnL9cHxzB5fR5gyffweWervcV4xPaMySXUqlNWbKO%2BUe1dvMBjYV4FwQRefLZ4fc6M2v8XZt7iir8ApDG824yWi8kxznBrGKvJrCR8RL20jUtgxg8pcby%2B7Yf1lAONO3waZDnYa7JU86C8OCRctFDGAwZGlT3Kk5SSjhW7SPnr3%2FK1Sw4LHeVwBwcqL6bVHMHCd4Sv4F8%2FcEc6x4EjqccFOjPFq8sPLvXXtu1dBgouqIQlYskz4JgpZG0lo80A4ALdDHVBNEpA2DiZN8z9IP4GxbxExlPOFeupgz6T20d65HLNKvIxsOcgi5uS%2Bh6s0XXTfed%2FtlBGmxf3haXDI6i1HbliH0XJ5%2BtZTFv2Rqq%2B7TSQwnvmXfqGNDKOLp9BCj9Pyrhea6aUbgMVy%2FVfo6srsoD8Y5gO5gDcBmSQ%2BEtCfl%2BVe2iSQcCmLomuLXdeG1sLMM7NiWeChrhaxtJa%2Fh2Q%2BXfRtAq87l8IEgvHyYsCOlhm%2BuGWPLADSFhDNto5XVnPS2%2Bn8oou%2FGpJFQ%2F4AejqSgp8Hxik6xe%2B361cESd7eQLeVIsAd0NfEoGbMWbQllJjC7o4nHBjqkAWKVGAXP%2FxLEIn1COtK1IxxDwAsUbm2zItaD%2FiKBnf1PSg6dbaPV10k1FA7P9r9zHPsSNgHEqAPBz95QK6rAV6yX%2FZv0uXOh1AjShjqN4X5SCY2TzmggOpcy2pZ%2B%2FNYE9pgAj%2BtwLkIAAmoWQoC8jz67od371MPkCG%2B0Vj6RG%2B2JoRPdMI2buBMFzKi2tvo3R21Wf%2BQqbT9nm%2FZ8dKECEE3j6LTg&X-Amz-Signature=abf4d8bfcc442e60d652151519900d31b6da7e0bbfe323ecc58357f32751a713&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

