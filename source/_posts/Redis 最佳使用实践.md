---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLSOJ5QG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJIMEYCIQDEy1BXxEiRg71LFmLADmUFIYA5Om0HeA%2B7lbRcE4COdgIhANrDpVtFb%2Fe41NUPexe7EMHBDeapn2KGOjebRHmorvEzKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxsvrrtHTPyvjBh8%2BAq3AOnJvuMGcU8vNygiOQ%2F0CszJlYn7po3SeZg9qPdw1bFWAcQeOIa%2FFwzAWThSr2i%2FlXjPMAooX1FR5KZcaR2LANOetBStTp6nT3FD0mEwvO0Z7KvkFhZz%2BWFW5mRvMsB8xnsI%2BesVlqfvXH08THR%2BhG60fel28W%2FfKz8rGaixKHkYRfFgWbMLjdcrnnGE7a2%2F8cNkZtUjMCEFn30mLD%2FzonZSPJO4mAMQJ80uY9FxAMLD5xP%2FljNFHX8NiSTpAceheATqO94KzYhugCr0hIqwwtDk8sJMs%2BCY0SGqSYUZhLKmjyqYylKhwvWdePks82OuWr0gI4jMCH86fzakUkWXXRdVKUxqLA%2FRk%2BGLp%2BXUQMq9cERWj2Tk%2BY3TiDYALGWMjQNfWuwhIhcj0t8mAnPBZ5K54Ir9fkKksD3Ijbe4jblDWE62HxykfwfcXeSBYcZ4yhBlSTyVGfuPSzgefFCI8QTxZA5WzgCeD%2FWLMHf4y1pLFMkK8ZNQn4QojFeKFNJKxdeuY7m%2FkCYgGuqFy44vW3iy1Q58bq1f7ywTBVafgyckRjm6OSusS1LOpFUSjfx32731eEqisVFEIJ3hlgerV0n9eHyT93YQY8PPuyPoLiVohLW6oF72qHXv%2FOA2DDM8ZLHBjqkAekRJ1fEfSOWc1q8RQC2bcQMLd186C74LZvyrk3t60FRa0ahy0J9ag1aAVs3Knjy4swOxRoFMGZ8FLThtWCtpsVj8Ty5Ug8icUZ9OZMkXXl%2B%2F2Ksv8EpwTs7%2FxBsBxbLfno4ADKKUgw5eTF4ViJPFgsZFh80wRz89X179aREVqMbc8u2ncuDL%2F2drN61iHD9ePkn5C7y9NbgzXYicW3leiQom9je&X-Amz-Signature=32e830ab45ab2305335c9b12f0f963e3d1eedc317b515bb92c1d5a6e4f8a5e06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

