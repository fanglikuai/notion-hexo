---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VLUYGJA4%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCEeQicvjGu1PA5cFu67gSydZ1r4otQIxh33FWJsaiDgwIhAOLANn2p4DEVMs1DqGqjm2zFZlhFi%2BbQH5SJN3pG4fNnKogECJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx25yWEf5JMIIRltgYq3AOQsHMQHYYVJCcB%2FBBrGs0tAX3vAbz39mvYKJ7YLaQ9B9P%2F7jE96PpqNzpdCCIVqp%2B%2B3LfH8TTk9Z%2FQ4DsUv%2BPUhb%2BVAXeCB3lt%2B9T%2BYemRp2H0hVk2ZrGHgnWml0NeBRbcH3hr93DF01ZNQ94JvCAGOsoqzkUszvWWc1notsxovUNhTQ93EE9lxroK1GywJOR7OT2OR6GrAR8nvXP1t9Mir4Du5PpeotvU6H8IXNPKXircrrwKJt919D35NR0hLy%2F0UGGOaqaimLjiDU1mUrZJrNTEPOd2DgEk5mjLLgFod2dHaDrqp6edpc4yixZAml8NOfgP6MmYfQYrXYcf7gQIL5ljUpMYO%2Fh9DeSmaH1R8fGaZT6u5nRbJxMsMO3VJJLPfke7WgWRm%2FDqwamWZBho%2B0RSbJ7wYNNnPD0jgTyIb5So%2FfyjzcFtGW%2FOx0%2FjwpsSgBwWqGd%2BLjBxZ%2BfDG8j8Iu3h31SgptqhOB1wpNMLKCbwvFnoUtp76ZyGm8U1HhUIi%2FgT83oer5fAiD7WQ8q6eF4Sn2ojUUpjSGFRNWjhTXyEQM%2BCAEbGkhqsTRLESHx08D0%2Fm5YmtE%2BfT0FWLhRNjJX%2FQaqsoGwSAfk%2FezyYrvT4LSbq5%2FM5wMOT5jDGm67IBjqkARgkQWKzbmtg8VMK%2FUIS%2FIcOCN1VhEZOcvQZc585TLOPGf7SBbGzy40kJ1b7Pzh58rEb%2F%2B41JFMqt3McYjQvJX%2FPve9E0JXA7h%2F%2BNRREgzdzGfwxhsVoQRNbqhl%2BeZrCC3ghhmqd4zTjUZ5lqcKoohZUnk1H6EOcGo5LvoJN7dgC2t40JVhxF6%2BB2K5z9VFJEBV45dkOWqwYibbcrxDD7UIxqCgT&X-Amz-Signature=92dade08d55814983b3645438b177bca6143a33725762e583625bc86f44a3255&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

