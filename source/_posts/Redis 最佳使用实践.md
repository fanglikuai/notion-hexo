---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTDOINK%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQC83WoyLZuTrMw%2BzdyX2o9J0vI%2Bi7nPPblGZZ8n1miSZgIgPnY6pwxoRynSCH%2FF4%2BBresUZk6i0mmKJfn7YtXw47VoqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeEG7yjkfVWDm5R3yrcA5KwFAH83j8PYjh%2Fk53zKIfKfh2b65BVLQZqRI%2BEc77modtwZxU4Rv004bx3paaU0YuK7jskUo1HWd%2FjwxXS%2F07pk3i%2Bgc9asdvkzG44wlh220AON3UI8eRs64lXsakucv%2FtG7uPVozc3QGveWtJxEwPPuQHDJs7hYcer9SnE6xXL8bVR%2Bl523J%2B2ViTlxUNccwOc4Kly8756D9cQvgkwICdMjK2emUsN%2B2mc2%2BkeQ7Jyf8oHu48Ie3ruIFDXw5CVWZ6OF5oCf1WPDE%2FLbdKml9I6qmGeHGBstridf2Svw7rYOWcRBjrVabkHWuM%2BJe5mAPa5wy%2FGJo8DaxIMD6RXSGH8gdjSQaxBhHUKU1HqIrLmhqigY%2Fe9wnjd0e5y011bZRGaqYHmFKYfwz6EM%2Bm3Lc7Cx0RvXbMQVnzLQMsY2f6yT4Aosbg%2BWWC%2B%2BxasHVpVeWt5vVRzBP%2FdXhwyrLEASnrEYYl9j73mkp6SyHTu9WeP8ia8Eu0KzJhqwnoWYkmSw6QKlNerBbAqVZEh8xv60S9ESEpN6BUJ1kHSDndmUAx0J45Y4ISwIqx9kC%2B8gWkU8B8WMxxNRU1U0feJaetRx3RwnnjYZYhgbXyk7kDRmKw4dENZHFopMKUWQE%2BMPzp7sYGOqUBGCJUsKiuwyg%2FlHJ1P8WS1dRoQqgeQ3exrZXshXAYYCm0Kv3sOba09Pw0ln1rIqdCq6QExedsvnJ8JPmzdzliUEbYXYjCwooMsSgBNKcGnwH8qXL1WFg98UnnanQ7voS6RFbhwXLAF3dTFMJaWVZ5MGIOcRtV8i5GEL4kIh1IHL8BoYhjijxm%2FWU25F%2FcgN3%2BGXzJuKxAWhfDHx3VmFROlI5Dexji&X-Amz-Signature=8618a9f2d086227855b91ccbe465e5de759f99d53643923972e8e96680a32073&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

