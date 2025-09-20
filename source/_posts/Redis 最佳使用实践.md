---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBNXEJ5X%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJHMEUCIBMvnOBBV%2F2%2BvuP1H4QmJgx5hnj5x2y%2B%2FLY8D2tRSbYGAiEA0WJl6pWkWVByZUXuozzMv1RZPDyjI56uK9anOyBPfD0qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH8Td6dRtR98x7sZESrcA5GpDgHwlsVwWX8hpbvm6jdYI%2Fj%2Bh81zptzXWAhIksKqjabZ5oWbnkAuHFxlo3tc%2FFVErzRIdZ9MNeaGFQkis8Tf%2BI498JVYdmGsWmLG0gAoqILHdtf82VwkO30j0CRAEfmrub%2FAdqMbLLu0uvSFYl8A51wuzQmpha15GL9wChYaSL%2FbLCjR4AonexkwIPvVszmmMhvkwQObC2kBhQGp2uchoyfQ0Ph3dd1QJc52F65DX9NEKWUstxlWX56TdX2ph1nwtbuZdNTBlV8sM3dJBf7I8TiXMStkgGy18U0wvw%2FvAZqXuiaEeOyuqJMq%2F7Bqyw0fAuJKiiuNNFl4ym9WFWybamuxu%2BD57efkMp%2F0blN5bxqG8K3C6FD5UgK5XxoGZxkEkdYtj%2BANDaFQC5415SenWjOgiK8ASLUuIRQj3JSl7Zb3%2FcSomk4JuG9N2mCq8r5dqiN9Dqlfcl%2BSvHZzljq%2BtdfJ0%2BIzlJxrhACwoZIrK4R3jOgRDqsI3RZ3TNBHzEB%2F%2BHgBupCuDP7B1v1770LLUMCnwAXyprMge7lbr8aKaQQtnfFrYyViYqHk6XnB6XKahMS9bTAx10PkYKe20fggjqjJhiP66gdBR00alDPqcDyZ3ex8U9VHB0ZSMJ%2FMusYGOqUBFuRy%2Fn%2FBvsnxxi2xPFPuVe3jlSP4APXQ66v8L6wyjHRQ89Osz3T5VU7iv6iZGcE7xbXijoA3SIFzv5y8and8p376bHzhG9ML84%2B27wncG4n2cugVcLRk62A4EpoadFT63UjEmvO41Xh6IyaczbLdymCln3fq8zlC7%2Fb2eVijEbs131OHd4OQm3%2BuD2Heqqm3RyveTMl%2BVCumIQ08EomxxeYAMFSE&X-Amz-Signature=778c4700422e4eaa12f367f36b84758e7b32e5a877caef2a792192d69f550be2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

