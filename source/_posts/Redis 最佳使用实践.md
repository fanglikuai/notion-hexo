---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN5L2GAT%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIQDwob59HAtk7OE3ZifVLoRZfmhPaitXAjHSuTExPo%2BTewIgDWhuzlUsJiG8IVHOFkuBu4eHcqZ21MzcT62XCij5Q5YqiAQI9v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCGwqWxJj8NpTLMKLircAwxHLY7M3%2B5wisboI%2BvJ7mivXMO8KDmnRQhMMugIZkaXDhf6lMiIky4tA0E32lMKQejndEWqCnkaa4NiAJ2danBtaZA%2Fxtwe2alqlrj6TYxZHTDxsMwX3cwTh9X4%2FMOoB1CMkDbRKDlIhJseCOixOwX%2BI80ND1LoH1P%2FSdkm9azYjnTCZIv9sqJwF1iPB7V%2BDs%2BFdYvu4IdtrCzTniNOGsyCw4d3SKHLw%2BWBIbAZ9DmV1YMLq9hdhy8J%2F0tdE5d%2F39c20ce1U7wdltxj%2F5NR7sLAaJleDkYAzfjtkL5TO7dfvoDePb4ybEWZEtctOKmPramc0UNXttgaIRjSRIg8L5ebrl0KwqdqDJ8oySOsjCYOyjDYqYmBky%2BuMG46t1cWSy8Jc5gl5B2N99hFQwbrDDR1eLpjCWTrDHjBSGJMpyzCZZEzrijmq4PwPEPHbdnL4baLi7R7jxTwvs49clXnRmsutVGCpOXhuld6S%2FPvHBrZiokUDJL8i%2BvXrY8UJ7nnIg5C8otpqXXzo53%2Br%2BqE9g6EmwWd8iQ%2BBI0ErG2MiECjwHMn6OlgfWXUBd7VWDlfFARWs7EEB2gE4hpmtCHohiQfw7lSK2QLtoopveu8xXOY7gO1RJengrS5yWByMJaqj8gGOqUBJ5n6t%2BXIEXwZhwHCSEoxWgxLNYHS5N%2Bak6t0wCrZQOvYWAy64Qz8c3S%2BV3BnT%2BoBRQVTNNamzCMLnYQwz4RMQ8nDHaUQdHWM7YAvpZEWVit7hLT%2FceheObCrkHvtMqMRf1moXp%2BuzNw0e5hyviztF9ZrxxL8haukXc0cQbPnsiyZ%2BAubZtKC1zfvSz9VP1iRjJPQrILqd9zkm2wu0TB4Cl0IWyBA&X-Amz-Signature=bd6312766852b4388630dda95812a72b8b6119223e8c6e1b36ddab7d8930bcc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

