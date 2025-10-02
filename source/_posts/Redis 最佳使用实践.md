---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RO3VNHZB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcwTa0ksD3BikxjF2Bu2s9pLyNSkUo%2BMHXQphScdPL6wIgUWtnWL0GYhOdtRiQ6zGiFOi2XKvz6A4MfkD4iQv8nvUq%2FwMIIxAAGgw2Mzc0MjMxODM4MDUiDBl8MRJzBQhh8s2gqircA3A8ybI9JzpDV07uBeFiX2x%2BqOML8zI2o9Q9Mz50VnncxWA7pkGt6xqT2MJOsGRwS5bb3FdK9Zf8lRAi7hL2PSH3w7iGyQxO7TFxxVgaPiNZddRpedos897dy2sQk%2Bm9Bvo4eaHm5d54DIKuJf5SnxYwhqAo0wb40jhFcAxVdWUwW8aB0OBkl%2F%2Fvgzr0HSSKmKJxEzhp2r3G2kIPJIhVhVWcEdFqncuCw7%2Fqn2JIahdaUL4f76f02f8ISDljPARRhv3wVzXwlmJQtCScDxZk0UPFKwadwjFWK8Z9yry3Q3ff78QVi6VkcDOA5Y7%2BtOA8x1euKQ%2B7a5lRhdO6R3pSaFL0xC53USIGO8REzSluVsQHKkxnJS4vbQLuBjXN%2Fd%2BLTfEus4NLfHAhLHPWhxr2bkdjeNcp7HvlSbQpf4XLhi5NL9%2F75GJlCfP0vQaxUUJr6B21ETxH04UYdIW%2BHSviWvYdrjGJ8MCfWiSaJ%2BPrvEw6Wtrn%2BmcxP1j5H9mfA5Jz56YFZXxURAcu9p97%2FOu9wctmXPV2UAjFJYlwqWWgtA21Dc9SapF6V73jUtuc6sltROVPePA7UB2kEl5Vmmro3O5TT%2BycsveVcKLVqqLEU8t2iKKceOvU96FR1eMoMPK%2B98YGOqUB6cxFCDe9EJFHyHBoshEOB19qPi8jhy%2B7S%2Fhf7TTGW6D0NrJcZehrJajKB8VgIQrevY7RQu0%2Bnaetuajhv158H%2BKrDBITPO3Pk0K0C%2FrnOyZWugb7Pg4%2BvLSKwbamu%2BH36gXKbHcGOMKirylUfXhTKOX%2B80On7du7Yf7mvgwX0xdUxWOGSSiSEf5V7xMUjKhHP%2FqkYw7a%2BdcTnO4pD3KQC1rwDMLU&X-Amz-Signature=9bca8dfeb94b7d8ab38116e3a4b497e6c193c3531675f9ed645295b2b9b5b617&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

