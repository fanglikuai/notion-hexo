---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JEI5GRK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIBTA495SEbqH9fqBudcujxJRr6URpFuKf0UF%2FZimhs%2BBAiBQPpMCx5eTnGQmZ%2F85AmwrCLpTuo47SKt2nCcN5viO%2BCqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDks3Rb6w2Lvf%2BXK8KtwDTvTim8%2Bt4V1oDfn2OlOVYkfKZbIzDpwKqI6zdm8FyWjYyouf8szQRhIsYkFmkC9LoRwRd5HCs951H071mSPGnNdn5TRNk38Tl8fW0pqxLNW9tcc5gUdzpji%2B1G0bAlAWXao2RRhbG1Jcsoydfb0F8ce5eHhYrdA21J0IQJjq7Kz2rTjrIMHD9bHyTi1K78Z9%2FgqKnb81vZm0HXg%2F4cLBuAcm0GrVGXeOszbev9z02BJgqvbfmRVMa6WJiaZTdL7T5Z0n68sW3kivEY8oKML3itXMemnv6RW1%2Bjp5o%2BIlPzfX5ywUZZ6t2qoO0RFJxhxTXmAD7V1Iz4%2Fne4mwKXd3SzFZZ2slKZPbenUpM8TLj%2FElS49n%2FxsP8%2BTRqZ6HAclkPXhtc7OI58gl7AhafvnBecT61%2BAS76Oy3QeJypZO6EvDkr9KcbgMeNTMcwfyR9nj64qyUAw%2BJfIAYX1sWfAKlzunjXnT8LCZbHcHm%2FscSKUiWY6k3yR0xQ3IWd8DPVcs0gkBw9ARhca9waLJKst6aWJVLZj%2FILrQZUbgFnyHvh9YQPlMPJZIs7NLR7%2BGtdPRCKuyUkC8H3CijsjRlAIdKuY1XaLZtjYAkMLnzkjfRcvO%2FsqPTiXo9CHLWbYw7OSgxwY6pgHxeH8ufMC8Qk2eIfShUM6ISco1jsgDYygUWPRsGxQDKB6BV%2F1jV5TEDQG%2B4VQW6HEKAh9VidQ%2FnyHvKJk4XdjKSAeh6l0Pdyi4EIpuQ6QhyyzNANL78t5XC7jGh3aIhyctgyceMjdWoER%2Bof6hynmpxZbBim4G5qMxr4MpTdXrST6ncjMjGLRElH3c%2FwQnW8QgV6KlfR9gogX7jJKqlZS7YJK41e%2BC&X-Amz-Signature=f920475c05962252126388a901a6335a54237977b57437c7f4d9d21fd317e0be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

