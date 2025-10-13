---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIMJ5EAX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPtAdSWAGO1RF9jBf3J8L2sJkbnNXJYfx3mWx1UWdxQQIhAJlBTlTIBH2XzGyOD4piP9pDvWmAWIP0JrC3Y7eB32w6Kv8DCDgQABoMNjM3NDIzMTgzODA1IgwbHAV9y0YGenWKM2cq3APxoUrWwFTGyy2g3%2F8947pajp3b9Sn91tTou85neM5tRe7m2VqgcURTw8nGk1a8J2HgN4%2Bb5g5mu83QAPym2FVhnSHrehJnupiS5c33HBN3P55IK22L1hSMaioj%2FL148%2FJNX6myZB0AIHJI8b4malx4vU45xeGX4n%2FOrXNq4RAEmtiD3IfNu5hV19pA7PKIQ1iA%2FQru1e96TapUbBsWJ5JewwF5wyURj3c87SwhGmlJ6Yc7J8%2BzcSn%2FTDOGE1lSpsGFNVH9GwQL6WhcibOEuoGIilUGxCfL3weuORq0Af%2BSiHW1ZguUYV21CjpgPFySlzjHhfDDpsWzm%2Be0LUVDHgMc2lkR8xJr%2FC2tGuxpSoSEfzbV7SR7aymrKpHgIB%2FzqHk%2FjlyxTNbdZ63Bv5A1ufj96STJGXHKdgE3O4Yr07sy92P%2FyEhfNnoXbvgXrPLLUwKla2pPt%2BpG3hWIT2i97l2N%2Fm%2FScsaNNO7YDiGqpDe%2Fecs5%2BjnJLQSicoS99SQxQdJGfXuu70U8C%2BKsqdTA%2FvVbr610ualwml%2Bj6SL%2BJSTKoW7ocP3V6F5jE8uLEYsw4bzUDabD5Kl96%2FMAcmxHEQIDD9fxTzXNalp5AMHBpkCfOdQSc0YxavoktLiEyDCC6rDHBjqkAUyDsKW2C%2B0YMMwaOlwJEuxQI3QVkU47KwyO0kkoJ7FHybAr4urZjN1wSky3WZdzFBXpUtToHhk8WgeDqEUQLmrAV%2Fm7riSbUzTy%2BIr27JS9xZcqq5u8fAepalR0EcpDVI9ccdksFf8TOdZ1%2BINC4zL7fZ3H7n5OokcZDiDWuzYSjzdV9HpggAiVLu9sEfGGjX12PO%2Bb9Te82A875awuSbtRvF1P&X-Amz-Signature=b35da6420f1465ce727c2401ce3020a7dc7ca7b40b3f7ba8d64707079db5965f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

