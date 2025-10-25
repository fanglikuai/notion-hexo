---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664P3MBZAF%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDmCb36TJTK%2FLGcb%2FUQ3jQUo%2BAAt1WvQbdsUL2FHGWTGQIhAMMJNPzIuvFcKcH%2FdZwZ%2FSSiJUnkl7NWrLZWl0P2VDtfKv8DCGkQABoMNjM3NDIzMTgzODA1Igxo6Bb2xP4PRgTvsPcq3APlQZ2GGP7ZiRTFP66XH0tcloiw1nnUUQHCI%2FD1aIFrH%2FpKgme5hAOx%2BCG4t5D3DEEoFxnXSBkPoT0SxOG4qOSWX5IwkmBE%2Bc4kxzLSGM0CKDl5b%2BFQq5byyCCHLQjIS%2F1IxQxtTQUrtXgaE17%2BrQYVd%2BXAIsv%2FfzL0wao6qfV0e6FZSKjMzcsUp1UEHsf04zZav4MAFjIanO407mZnucaahzpKvRjZPsgBlJ7TwM9fftS%2FP9cPADpa53fv5YCttpBIeQVRDoZ%2BMqubHWzARISFIWgF5HtCH4Q%2B0Iy97Y3H5Pe28zbRAkc4Q7JN%2BGTSzf%2Fo9sqaY9As2vkghPjcvBC8SqeM3TNJ%2BxTGhJvsd1jPB6DCb3iVbH51bVx7At65ArYm4pmuLMRchDevtcS75G3bwDifxRm0cHf4rfn5fPtDfzPDr3mIn6fC5PBHP1wRCVKNQlMBlj3ry04AhpmUSuyB7uZYN2hbI%2FQagQc1Vk3jI%2FYTWNkF6q1JpWjfVTSP2LAR1KwzDL2GMWIvj1vAiAoSI%2FYtxyUfZbznswz90A7xRm8x4oI01rXtGyq5lYr7z0FXZuNkEQpRg%2FuID0H9yiM0IyVxloJCW%2BNwQZGMF0oJekWZt7GHyUGaL6ddYDCimPDHBjqkAfnOgb2WKVv7B27m8rnZjn4JU9tFqiijBIdGLnCm0dGx9llbMuydnPv7R4tM5LOxXTzjhG7q7i%2F3tHBTC78yquSkiYIPst%2BS43eOfgLhXnqbognG5nPqIrxm%2BscCZLzTxzsUmBSR8CNzLJvL1nq0eHcHh2BCk%2FDjZm7rEviaVsrQkGFoaL10qAF6nq%2Fqf%2FrBJ2ULQZrMD1%2BPnzdTZl%2BIhBkHtNqx&X-Amz-Signature=657aa2da814fea5037d0290c2a9afcac8e70eb7b076aea7893517b6f0f4288de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

