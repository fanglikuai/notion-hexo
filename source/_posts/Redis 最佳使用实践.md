---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXQLL6Y7%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDb2a5pq%2BIfxn7SL73JOG8H7xrmtEMpNpbVSQf2thIxqgIhAIHKPZgNJPeOguC4ZpUgmOdsleGu9vz%2BK9mRNnaSVt7wKv8DCCQQABoMNjM3NDIzMTgzODA1IgyDUourK5ItO74aAxkq3ANbcmlLZ3U3lUom5n3%2Fi7b7z4PQKESc%2BDOWI9nu4SNrzDoOzYJHbxu2h1ydcoOIrfLPWbQtUSiUUET41Mt5BkzDnVTCqXhimanOoMcUFfDXWN2j4swBPY6%2FNoAVYdGmkGXORjCdpx%2BBxcyKeHIMdWVVfRxYOIZpLQqrv6XdIZ4alZn5mwci0DgolSIm5pyxzZ2O%2FZ4cjLjmZ31DNsrtrIvnwuTtIrmhIfXXNwz%2BP3wtnxJtj9GN6LQazukO6EYLGM7S9CWkE6RZ4SHEyJPSgSJRTcZt71eZqpGW%2FBGd1TsHTNrcVju1VmKmds9xcfM0GJosWeO%2FefT3iKKHckL99v%2BvP0bIOEk71Q8QUzsYHX806yf2O9s6vva2EYpcOmYNJZxukRNG3Pk5dKNdEme23qRwu%2FCWBpqgF%2FNDuYH0ZNdUVRp5YjeeRBlokcPHU%2BTWZ6SfCWvWHJSKquGwj4Jh%2FboXgIml5MYn3c4cNQwHwIWEH7VKZP6ic81oHEWvi5%2BHVBcCM%2FVpw0oRb6sny2BZUo1eNaUlXQjmqrHs0KlPvAesjydsW%2BDQqCwDvZ3tXLEtBuke9Ykr1o3jfM2kV6CrGyjBo4SlS8pDbOlUe2dWVwh4rLKhrRd1PhNrVEiTbjCKoobJBjqkAcBQTlaeX3U4hl6ieql11svCUF%2FAbZj4f7XOhyKvb0QsUrbKSJpF39g6rhWGdbV1uiNhNIWtQdXqzMf6zSjbSKl0Vv%2F%2B%2Fl9sigp8aVsod9HSm0kPGG%2FOM9qjwuEkxP3lifg%2BDJ8%2BxE42shI0uuCRRT2GosgBnbBnE%2FqA4X09SdJGXUmp4cjeC2TXM0erhV3naBd%2FpVWHpcrvypKO7BzenowV3Qx0&X-Amz-Signature=3a3e1d2f64a2189c8a56ad9fb4c51b74270f5bbc205ad494fe502869cb7eda69&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

