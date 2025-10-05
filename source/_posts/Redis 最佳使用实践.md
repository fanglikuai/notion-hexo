---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKWK6XTG%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGmBgTn2bKCEBUjoSkb0tDPGN4vu6kWVoD0HueSWd5DBAiB2P6BWhGuzy7nj6Y9mkQJ6YcCVOu5LghJ3z7xk2%2FYfair%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMcbYhqDKDbTa%2BV%2FOVKtwDYkLVu6snXY6az73egpYrSfbMha4lzA1Y05%2FE1x0El5JjAJ9Uj8KFwbOWkQV9C39wJoGPvYvE36%2BsxyOeIMF7C05%2BEIUKt6GcQcvoubq8%2F2RTFaAHSNQ5VGC9v96zzGfoNJ7DLbBiEV0C7z4R5snjamHRczNox0qroO%2Bu2ZRWu%2Fg%2FKwVTHSCGy0gs3Uvx5mHNvwakqFVVvorocGJ%2F6LqFtwkjJi1o5Dz%2FKLJGrkjh%2Bubq%2FGFgWgbpYaLdf99lbZl9s71tuiUMUXUWziyvxO1uTDVdPTvy0dLdHDjsyE1Oq1iTajz5AWtfrM%2FLrr3GKrxJyE612mCizZJdrNXlRIX9edhCZ619Ru6g5WWcTvfZCRjdtyfc8Os9NFGfKKz4uLSm6GpJ%2BuwmqEvmMRIuXwIXbmBtXg6SdtZBSfoKRxlKP869nSbjU2xvLtkEQBLXc%2BDg1r5VeblFfKAdPf8LkxlSVbPXgmU7Kd3qDKVPypiiWmNY7WuMyonS2ZFbtcaJ8P9CifbSDHR6w8Tt4eUNU%2BApZznh%2F8rkHV0dgZcMmvKMjA6Xr1ZTdPM4A8RQvOdmmCU%2FnU7c3JSgEJ%2FzDUg%2FubqFBlmtyTSEEwyxnN8Bf6ygo3o21S%2Fb%2BfxvWOuVA4owheiKxwY6pgEe77%2FjLOJdkb9%2FMPYbEOIC7mnLW1XIGf0qEgJXRBgsGxVi6s0FcPRgLVfIajeiBtFQTIyAmjNrBXCjMsHYQdOMRU4db6pVvtquf13jH%2BdsGRS%2B9eoBzOevZpzwG3MoAw%2FeETgjyDW9C0aW%2BbCaXQ5w%2F0YCRSazIWt2o3TYqNzk1T3bej9HMWjA6cfTvwy4lujF7WSA0tBUw%2BtxmQ4o2VdypEZTBCGj&X-Amz-Signature=dcccaa040a080a62d8983381c645e324d7104356413af84215ef36564e33426d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

