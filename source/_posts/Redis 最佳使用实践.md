---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBOZHJOB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7MlPTL1VSXykEF5ZsYzsfhkEOtL2j61dFOBKDZZ6DMwIgEY3yqOq9yhuHlKigQU9phP39GI1Ky2N9eunVoFXhYXYqiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL956DbQOKCu3P5RuCrcAwT5oU3ik9h7tzfx5kYwsQia0w543a30lDVle46YOqGeSVDULGTP3rdmpqqSE9YoJYWhdlcK0RKgQO%2F7YXvF%2FCCkhJCPMP9Cihg2NsDzcu%2B05ZFYKwZqCraRM2DOWUWzMULz3laToAZpDNNAHgkR8M304yqX9PflveeSn9nffGBpd0mO%2Fr9tPJQ4oGPSW2rlFSZJbRTbduLUMKjjZ%2BsibNn%2BkB2VHbCqB9oMMBGnqgOrHdox%2FboJLfzzIl1GP2iG7UNqjSApiePoJnhMNPUMIMUrNT6J9qqwI06SZhUBq7fvf5EjcDVBNfIqkx%2BEqjzprfaf0FqTBFfasmyLJp23z%2BGamoi8lABpgWGVxCRUPbo1P1u9qKVzyGNMsmTaIAy22FK8cwZDl9SSFxpmjGQtBQPsuJB9cH2NtSwBcuO9AHQ39X1eEDFVxPaN%2BtjbVK9wXYBPr2hMrQooTgvruAUmZzOn8HYbZ7ymC04WLlcIiG%2FVwmQ2sOjmSDv9iaS32L6wjOgoWHPF%2Fd8OWcQqeztE3Q2emjZ1547D2VF%2B8C3PpMlT1MN9wswBgMyCCIU1k3WWTWvtVj23JqDjUDFkkGspwyzgQz9zaA85tyinWJB80CWyQqpr8aq52%2B3E4rSAMJbtt8gGOqUBnJljqTg%2F0JtwbKNgrQdf1XukdFT3Fei5K9NS58%2FBt8CvjES87eCKwPIkSnjqwWAyzNgSoHkSY9zcocyggoQEDhrJ7OLk4zJNmrrGHFspbYyKUfD7MkyRtR2C3tFK8UnKjKmKwsKt4uwgTXu1Ja65Gyckk6ATVbDntg8fnHsj0giWa%2FiAuxGvCLXBiNTdY2Hn0xulFDpmPD9YDkoflP5gxRnlH64Q&X-Amz-Signature=123c9acd3fef1bbdfe2c21dc37f9216a78442fa7615352eaea0cb693aff31cab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

