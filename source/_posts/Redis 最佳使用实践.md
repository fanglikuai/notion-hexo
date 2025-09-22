---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7JQT6VN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh%2FNN0VHopUgYkOzfddziOF8ofea75p1VWq9y2pEX9TQIhAJuOeGuXvmSU2ey5s%2B4acrZWkr71v2vqJmY8Yb5hqHHxKv8DCDEQABoMNjM3NDIzMTgzODA1IgyAGdQpYnpcnf8r4KIq3APg3MfAFpd1iBpMgy3%2BP14kJA6cL2ik%2Blf8b%2Fcvv2tmkWo4P2gjKkMNHb54NjiEgfdraRmPaSvF%2FYM%2B1upwklNMAq4riNjejw9YABuyxEK%2BpIFSyJKGWBcG0Kh9rYSNrq1Oe%2F7k9jBg3rBUdE%2F0kPrU5M8X38IrPORy4ceZlTZjlW4eENdLpaXKl3k8fVSETVkv7ZPE9ZpomrSmjZtLkccu5Ag5H2zbhSf21dyZEu1W6mOmeQ8eKezi%2F%2BkiCk89600iTqnKWyOhUobYHYAawFaU8HThUEqA8GfR2Rx9cfZoU5io5VuLQcsmmhYV5N9ROieca4d2j4Vzlf0fKESBHt7D2jfb6Wh%2FXv4qkIycTUMiu9plhNe4oKrpNEL8uWDyQ%2B7J%2B5s%2FDdMmSMmrViSMh0hTZFF0kcQSSZXzgiTMatngS93rEntEuVYMoe%2FkGsiLSWo7eT7ZvrSqyhQS4eD6tMn6cXW5bkJaeOSOcBNjyorptaXMCoGnDM77TYtD2IDUunmusXyYuCjFR0gmB0capRMm%2BMdLdTDoy9mjyypHBt99mKfxEd2Q2gasNq5C7lSSfcrU35Kn%2FWFnXRO%2BRSnAvmobmf1oTruGsHupEbpemvUSD3yDDRgHwpkcSOmJKzDo4cXGBjqkAaZCw0emJW1o6f8WZLJSe8FvOrGDQIETjSlgTHndAlDJQqJofbOURklenjYoqX607LCFu4nDDxCSnwZoT0Ob6soW2QPKvxj2j5gV2vOqIk8zSGJD9lEZMzeRUd0kCTMAQ7S1zZRdnvfU1Fn4nJserJ8dXu9EqlNxWf3ozQ%2F9Bvo5sBq4CMcl6BOB525CInkaH840lCcpXYZE9XPtX6zyOcNkOX7q&X-Amz-Signature=4c2d5f57a166bb3347c83ca1896a8dfae46c6cb000e021c54152e6d33169821c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

