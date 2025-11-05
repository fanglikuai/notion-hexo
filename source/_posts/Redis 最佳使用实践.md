---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWJJUSI%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDT4aqlwVN8FRGH2rWYRJ%2Bwc6doyWQR6YqkMJZ16Gc6AAiB6HIA9Cyg6nxEd5XNijT%2BvjF8gtJHe7UUPqflWr0ne9CqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0cL9K%2BGltt2dx2BgKtwDOjtpbITm%2FGXpxWHirXiyGwty1Qf26Vl8GXfBhdsHAs%2FHl0VWhEsFoRETasllDZRSGFCDy82r3Tz8C0%2Bahm8fO9RjDCJlCtte%2FzKCIGzd4Ml7cZx85sY9p7Go7s1B1rsBI82juAWZsOFXhY52kfM9ntyEqtc9I39PscHK2MdCvHStgtEC1xw3HX8l2zxi6GuxL1gOsQWSemFUoDMSHgsR6K%2BwIsX9tgZ1Lyha2Ve0uAn8PVwKxuwpjscrD4E5BTsllQ0CJPpgrTUpjjlIVJ5ZhMDb6cjsxjV0ODo0wCIeOcbxH5YngYmV3OtcCX1AiiyIhf5huQrwHLPc3SxwDAMW88v7wi9M6t5BBbVfoLdYabdsyow8JB3O7icC05SCOdEBRZCSKdULodOcnkuBPnKP5%2BwgJQRUxPvoaa7%2FjBJRiXKED2XW%2B4koIFTfbtkttV%2FvmIEiMyYk9mAN93FsECQX01ujN1h7qxcxNdN5f0Avv7A%2BnnKriRC1oSP%2BWQF5qXw6vrXfJeiLUHzTotop2deH0j2pyY9uBEyDCKu4wqryQoHDRMW95uF%2FhHWe6beOiaiH3H%2FIRCusQAYxPEWBGXVuLpmgn5B7mmnbZNKsBUx3xhOe9jjRlUfcE9PsWBgwkfmqyAY6pgHeuTYfiRXtjfrBIP44cq%2BD%2B81%2BG80y8De3VEqbkw5W7VzMic31JwV1giF4MkNhXPDHyZLqJgDInxZaM93PwV83irUenQ%2Fa7MmcrTOpy2411LYXf1aFEbpfJI%2Fo6dJdYmDgWbOMlyZNyTCGKQNPZRrqe1pcOqha0Y0ectb7Lu2FGJ8%2BP0EeNJqfFpMzMqRMlMHXzNHPbBISFfpSJdSEh9RwftfdeVSO&X-Amz-Signature=667f8c833cb4286199a5cd637b9cd8b3aed3014c57ca3793db3333c8345db62d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

