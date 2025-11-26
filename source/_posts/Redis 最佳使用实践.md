---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U763VWBO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFL3BlfR91gZrkq7ldPxKlZ5N68grefRQ1U8KvtrSlwyAiEAjpLdrtLdAgMjn5ZOGlQnTk28x2UhQimZGZCetZGuAGMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI9z5XXu2DhhS3mS%2FircA%2BzvZCWOYkrhzUhA27%2BacHH04aSRVfqpsxbIkpLa8Y8BO1iGwzcAz4Dwo9DNUxQRKjGecrUvr7YbrwBbEI57Va8Uw21ETOw76C3BF3d9%2BR2TtfCfu1ch12GpDR1ZZbUzgqM%2BWrqOzpX5cGoqJRV6FsT%2BXZun2uMbME2NPa%2BZ0xl47hyarb8DK7FmTVkslkBmUE8lAYMUH9ElG%2Biss9UAUjF2NH%2FDDrxFH%2BBwqNLQh%2Bv9pvhj%2Fmn204d2bGjSH0%2FpDXK2MrFUH3viAKDcWRwuIffj9GKhBeCJB%2FJiwcqAxyG3ZNc6uWpcioryH1HsFvIQcAqEWWWwdHl0yz5esbsU77cYDMiD8nWe%2Fa9WvMqLUxP%2Fs9sR5nxXrJ3T7uKqgAtsWVzVXQ8d7AuMuf3OfjQNXf2E%2B3uELZAT7XaaKCIlT5NQf3fyIWsLC8MSjZg8kucjLB38G57xmcChVQlW76ti5pG4OPi4UGWO8yVnc2eBAFeoaxRjuPTBfuAhFCLJDDwSnRMiWhsTCR1n4WcH9X2ehtF9SrOkfi3Fqo7%2FMEtDJER%2FKmsZTacg12VUcLKBLsQYBuQu7iEZ5j%2FnURaFyOuWgsEf4ao3wnlHSvTb9QD%2BWZgAPf7scwzB2YdBpSheMIyonMkGOqUBmvSSjSNENQDW8RTqulRIo9Cdud%2BiEeH%2BnLQPY3iTrujz%2FJAaZrAAYGgGL06%2FPJIRGzideGHtk61NwfdMc1bJ58owJalCiz77wKxxc4PbJNewRwDsHfhOn9WHfOjK5my%2Bxn6i5YD1HdxM25Dl%2Bv6M%2F35e7wf3KXKlSWpOsVayCIWhN0nMqVyqHEu3kFqQxSa003jF9WOYGN%2BOvFs7wQB%2FA84cxDtE&X-Amz-Signature=5d257f2c35e2bf097c1794cdbc3fb9d4ae651f17a6fa2a4749bd686970588760&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

