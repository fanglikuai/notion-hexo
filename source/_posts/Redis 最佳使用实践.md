---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZUQNRA6A%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIHsBZI%2FdWcLVmm4sDbrASprF03DDJj3gv%2FpY7Jd16rxjAiAk57qt3nlBspL9GkGgzZnkiuD2ZSAqn3l03E%2BEb9Ib7SqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMb7tnqysV4Lar%2BOKqKtwDLtjY5ufDAMFD6PVDLG6pZAIc%2B2UqC4Rmo9%2Fu9ayZDl22bfz4VdBo6KfX2BqxqzSH8x59XhIH7GNWnsO4VV%2FTLpbB2whKMxdmdZB3UHh3g3oydcEw0EDE3o8JO4vkKQkOcrP4qIGGqgXA79xL2204zHqWtX16aNlOY8iADWg94MetcCdAd33VRCYg1AZDY7oHy0fbik7Sws96mIUegQAtlpPvZ5zKKKuud2KlUUZamJMAQ5vtax5ZkpvEhj5IqJHdU8ByuOtDbCT%2BhkUH2wCytarQA%2FohWxhwQ0EVlMb3kJ4dBLo5zVvtQwjBHX8hPB0ovHow9Wg4gpWnIs40ZXtozQtyuNAWaIcIWyzLebbdaoNu9LWywXh73CL974wXf1vGDaQ7EtCPzTZsWZJmdU%2BOGw7Vv0kq%2BQJXJJ1DSVK%2F5JlazZwhaJ9yrfpLIW70N6i7IywrHvbdPyVWGF01MljY4G8L8fnrSVrRujZeHTz9Io76FtQFVFJeGgtt5Ga4ua%2F5pf5vNRPv57Qtlo8ypw0Jh57rkxQW9W7MNxB393qhmvh3h0XQ2kZY75EwBEYgedo4dN1OsX46odgy7O6sAVncVoUY0Plo0CjC9RWcITK6fwg8BnwHnUh1jvCONAIw2empxgY6pgEH3bwGL81i6n9233sC4NxXp2eHmdplnBp5mrGYWxs7wEkxiZURfVUhaceDc4cpY0HcHzmH024kUjFycVadAQ7tLdWblMqxtiat2b1hiTEUoHu19Jd2vvDgc00xJeiDrN4%2ByheeT8rODZ0sGWv1UkaEkqYelCvlcnFmA6dWfDtLI2vnoC3ROh0QaE4N5Ah%2BQ4sO00mKgv3QcCPotwuQ3uSEKYTTaR9E&X-Amz-Signature=3fb6ee9066e7c76a68b727068bddb0dcfc3801d218b5012a72adca60e774ed17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

