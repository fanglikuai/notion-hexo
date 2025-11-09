---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QC3RW6X%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIB5f90S9S%2BvDq3WeOj1ZDIQTD02SbWrS7tyfqBXdUj2FAiBENPp8kKc6EVJioAQPanORQQOeV1njrs072uItze4EkiqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZwDpRU1n3UvtE%2BWxKtwDKBWnFIokcZZ63P9jMBP5Nj5ufW0%2FFTD5ckTwT%2BrDWqo4K0L%2FIflVIMxQNm42YsDIK1IKVbX3275%2FhnRxGmF9o3pK2mxEbDUNtmD%2Bz1FY8zdjpxFQ3gPJMD6oiLbm8MhM7ZduZxnctkGJs5qclNHfQVhtiBkPguzy6e%2FXmvgY8fANcnkKDR080QlswHCGNZ6EMxTxsF873gXvUzS4PqQNyzoxDV8gUFDmbKqScczWwCMDhjFShFwWJUJ4gnsUTVpY0xTq6nRfbq4x7UFujJnb9N20Bkf4F7U0Q9IjQkQk0iKEtfMOwhMgziRh0CP%2F2%2FBBa%2BPV7SCGMEQHa0rYaolW67ArvvrBCq5Loa0YdkpVdKtV2oYKV12C8Cm57NpTYfE%2FAcO9cOYNu26zG91%2Fnit7qeWDQClKxxOEvBN5%2FHuhP9fLWsgKZoHQ1j3IY3Hs7P13IDLH%2FLkfMyOGQhni4fWbLa%2B5qTUvKEngBLmxQPnMpcp%2FJk3u5%2B7gBA40od49Pql7M4zmlo5Jxk22c198DZCdUdMM5YSWQZxY7aZ3pOo33v%2B19RFuq0u8b15ehHmncU3hE5T6AjYkoBDi1eKAZQEpQWwhZCAuJCMXvSjHYFHliwdLSLNjOaxQsV9Tyi0w9ZHByAY6pgHTMzqmuYjOLZpsIRa9we5M9%2FSBemMMotD69qnbU0zwpuWcRqbU4v0nmR1cN%2BJIl4bH5kPZDF8C66c9liVjDHpZnTfefKAx8P3aP8LHvGQ%2Fzn8qsUplUwqJj5%2F9YyVmEfAd68e1mbP740K2fl6QW6AMy2rnLfR4pi5VPrDetdOYtwNM4OrQh5%2BNR%2BCFeGQmLA9mAzzIWSrTQ4o1CO0udt8SNwg4RnVS&X-Amz-Signature=19b11a0009cbc303b946a1a2bae3beddfd315c202568e445166aa0a92b11af73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

