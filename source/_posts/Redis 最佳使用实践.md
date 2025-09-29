---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665Q7HG3LZ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCICnE8TUCpLgCXyFWh1tdvm1NJ%2BuJeQlmIl8Q4g4hhYOQAiEAuYBbdiPNbTzI%2FNykqbeBMRbClLpLOGZZj%2BgyD2x7CqMqiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC8WupoVncujYseEUCrcA%2B91FBdHf7Th2ZwXLz2qWD8ZGZhXBjWv%2FsR8QvyxLkdc7NnTLLEe0ogXGqHcdPlc%2BbdYDeZKd7grzbbkaa%2BkKYzO586rAWbquWHT%2FEUQ54hwm%2FSzNZfjGWSo40MIY%2FUa9WlUuDXvpZBfZvkQtFblwV%2B%2BnL%2BKDgGbXzuIOBRk5erJab%2BftVGE1O1rCvgZoAw4mDsnWSiI9e4J9bY%2FM11BGsrlmeCQF6QXM8tkga%2B075CB6Rd%2FHbbbryjUPk9DvwoIXSYrO7JAjYgOrpIuJuX6cROCTwVxI0E%2FwqIIPFFBwjUFrABjBI5e0zhlUyX74%2Foy4KUF4DJIcicCoWs%2FTEcgRqzH6VJjTdNjJSkIsa9qfnm7W6kvGtCxO73MpzFxx%2FWPjoHD%2FlSGxGkLoZ3zp4AmwjhX5fzR9%2B2joe3%2FMloq7DJX0kvMLCHFzST6TM0bjzWI2dokqjMpdtO1F0lOlr36tinbD88aGJ7kXr9jFEPu5jQTcNo1Pu1kJXTtJyJeSDSZEpn%2BVMYm7p17o38dS%2Fe%2Fr2bn1kNJNQNs6vNd7I7CC9GMQv72QcKNQxjWtOS1QIsSx7Ziit7QIXBTI1uEEw2hhF3GPUoEoSbPABiE2w1uzHTpajFMROogjVQHR5SHMK6z6sYGOqUBwSvqEAhD73NhrDZqmoejU1W6cQ5DxL%2FNgY7PyWuvRkreEv8uWRpE9KXqjaujGD6SsNy2IAEyW39BJbjIjKrEvx1bASyOjxeAZKiD7LV6DTUL%2F0COPrcxnb6YUN8j41p3l9zyKXQDF6CxZRaYvdXzJwQ9GCDTFtNV%2BX1e5f8Qar3KndZ8d16JII9xsBx37biBiyNvsG2nW%2BZzh%2BWPELdiPDPPS4Fm&X-Amz-Signature=df0743eac965b47691c676cf96656064ed5c3d03a6d352d82c3992266fe320b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

