---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2AWMEAW%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAmDtTT07cXlqwoarn2p%2B5A3fXa%2FMQQl6QBtfvshzJlzAiEAgDICvF2HG1FzQbrqIQ5kEUvkFKWZEXq8BbNKTMhpkCUqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBmVNt8OR7Bp4SBYNircA5sgsxX%2BOkzwFNt5mhh9RC1hVbCEIdd1v5VR999K1YXw2%2BZluo3lbDpRHpz6Aj7ctL85%2F3UwsD8NvaxnAld1EEuLzyzpisp7J2BcvZYCMmjozljpsdDNQZ6VV6a1%2Fl8qdyFFoi2jy%2BU3%2F5scMN%2Fo1v9fvYOhyD1PU4UlYapi0N8L8t%2BUYR9W61Idp0b9YK9zVDGpkVdr%2BkghvKLMAi8h4mlPttdatSqf1iTc0ALnFwnvCpdFk6PXNTbGzPpyXuOZiLN5r%2Byi%2BEzGTfF7kKtfR23cyYQ8cfa2xbMgwd3yJaAZ%2FmMKHqj5k8m12hGj4DkE0bB6UJZOk9H0O3fLPrfcICectAV%2FGxOg1GkIb5n16jXK8GPe04FJeFERiN7rfpOWfcK6YJ1OeQ5CzO9m3do8xQVo5qua83OChVWjqoDYybJgAI%2B1l2MrB7ICGeQYkp52cJ9MkLZHHwkW40GaPaBpQ0DmMvU37ez%2F4PSoNqCkdQM%2BVAD7nyDBg9QiImrLzExAT6tYtOMHGHpQ1DPIloUFKBzTNPGPmwWbTGEvblQ8ud8YdUGmgtYtgfCw%2FoCwRXn7hBuEjih9X77M%2Fieaf6yGMkhZl2TCx4naBI9SwMZ%2BmwDVkWaT2JZgnrXjE2VvMMO7pMkGOqUBop7mRWU9q6z7H81T15LIyNWfv%2BLj%2FtNBEsFadPpQhCqR1qQUXokGMIrPgIilC%2F5xswsgdsHSsKldZ3R0rCNwnrr7T8t3BzCciKpa5UGqca32b4SEfqV7DYcRVfkHMfWoJVRyhi7PeI70jrx0UoAadpn5%2BpUZmKnpp6J6E0Ky6D5%2F0TWRinxXnKqoGJt9DEVmkEDWDS6rvRZU0wIojwG4UIaV1rK1&X-Amz-Signature=3b70324a0eb0685cb42e7af388235355eef5a506e4967f32b9dad1612876ded4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

