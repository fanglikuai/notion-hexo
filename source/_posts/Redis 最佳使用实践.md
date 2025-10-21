---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VJUQRME%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T090101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJIMEYCIQCC6frgrB2IX9WPpkwezsMWwm02cX9noyU5jLGFQmMb2wIhAOa6Lm2H2dnHYiSRLYPN3NY%2BXLRGfoSybCD1WFQ0veKiKv8DCBIQABoMNjM3NDIzMTgzODA1IgwRK7bE8i7m2Jep9Ogq3AM5fyHBx%2F74QdnLYH2oK8DnN5N6xgVDgL2GdzvF%2F88LooDq%2BSClv%2BE5L4DyUh1MCNREgZfQuymIVzcxdigd%2FjEx2qDLwpTAL7LLNzhXQw3aOK%2B7SXQkJIUNE5piNMz8xXbJKIocsECMto7ct6r6r7BPggOmZdi5TgubQt28ToZxy1lGwvOSAR1rxXQ7KrZkIbdw34UOVRrf65izZVMtPMbfxNMPKsLrZB9uXBfmZG%2BRL9HGHQhwFYH%2F8YXFYQ1sZKzRKklYZhufLqYlMMskwCEIu%2B9OVX2LYwhiV5q5JHLyL6VZ0boc5RcYhUa4pfW3qFTYXJwYnZUI249ZUBIpFPBGPMz0kyUm9bvcMRodsIc9gPwg31MWDaAuZbTQOff9cE1y7El%2FOhCMLvLlj4IWP9iwJLTfrlBzGosKQ1SG4ChJA7MW56v2hABRU%2FunVIu1CtxOMQjOq7MQsxjY%2FBu6H2r0W%2FfI5lvaXPekMoqserJG%2BM%2B9w2hxMLA7ZSp9Aj30KDG0JMftQUHH2uQEZR%2Fq8mW1VcYUkYYbeP7vtVqqRY2OnYGdNYwjdesf80mFpAhgoaES1HOaNTM0wx8qHgYHJ5gqJbSHlDzwUfv4TdxXfxfSaT4NJmLQYoMVsN%2F%2FqTCElN3HBjqkAX5zdPPWGCEDXkpcTOugkB%2BGeum%2Fa0%2Fgf7xjdRwL2SfPtroJDxdqtP8mct6uIa0ufou1RonoWZwXiF1OuFnr4NXVd2%2BMSP4KBsHhAcQN14%2FQNiN4rRnx3YlMftC2uCi3kxzv7BRz9I0Pt4w7JETouYVh38QSyJv41%2BrQyu3s990lLsIH8cCsW%2FNCnOz0u7OtiXsN6zTifjq65cnKC4TZRbwzgUgC&X-Amz-Signature=81141c098bc7bf595ea52618bbcb643c5643a9f9eb2cea5f6b98cbf0d1787837&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

