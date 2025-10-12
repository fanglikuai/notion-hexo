---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7S2YMKA%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T060056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIDuhl3Y7U5jMtXU%2F4zyS3ycHDkZWbWE3bbAzALMussCrAiEAkU7%2F5OfHloEoQhIB0x0ChzjiSUBa65FhZf1iPO44a0Iq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDMvqm9NLdxZS%2F5BxPSrcA%2BNRB4YE94Sn2%2BVEVSYOl4g0fO2rsZqKYQlIzjGvaPwUfxfHzWqYYnlxHxcqngiB2af6OErAsufJAA05Di5htY%2F159p98tgpYypJwIXZYdU4mJbr9Bbk%2BJU9aqTEULkoa7IggUiRAs7O9xaX0O2GVnOtJrSvTskn0t7%2BEvE9iGm%2BhM%2F2j7rfQgeRZPZ7XnpHYflGbqQ8CGGGIpzetJREwVlH3nbkWJJwkO2s4GGea%2F6pgmdV1lmqzXef1B5ZUpRSMF3ElcWBa35TxWjIzUM9TgB%2BaoFkX9dyi1aT8hMjZhh%2Bv2eeof%2FoSQPurXXtRmnFISWXGQKdGACv3Ba73c37joCQ%2B2qCQu%2F9hKvtcqSyHW%2FNdkItFxfuwPdOwb9nBDMCW%2BDY%2Fa8e%2FW9u1VDljnKrD7wrhu27iBkJnKeQbubzTAdayQ1INNOKZvDngvEdDJzaGs88BsiKlqFw25IiEF3dgZ5JlQLa%2FAj9gnWcETE3UfntVkqSfQtUADAty6xV95gqSSemwRAcPIbJW0wZhXr7eN9Oy12d9Z9336WPvwOhrxLldQQCTRsnnme%2FFMn9PUp5quK8rDwUeY%2FdazYgWvFv842dVFp7yw3V5uIK06nk9kUpmQ0PFBjKCQxK3e8UMNnNrMcGOqUB733TALHgNxZAUAsf%2BJzBHQ5l3hn4Z4UAjJ55YOLl2YAT%2BRoSePNfXQ60kj47sYKJjXJ4hcjw4eTW9zTYw5Rr3IRcB0dLN4q6wl2yugD55Ze92NZo1qXttqt%2Bsw1WQ8ceXTx2DcoUfMnuehGRRAcQAeUG93Q9IJE0emWJUYjDxFiB6Gc5AXiQ%2FCnc30WJN3vgyuLz1%2Bl49H4S0ueemVdP4YAN%2Bi8u&X-Amz-Signature=5b7a37490cf1dae6db01f45f983c7e71ffcb29f2ad84dae9ce7c60766192ed68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

