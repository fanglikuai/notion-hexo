---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYFKQUZZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIATOLgUMIExknlduCGzVp2sfbLncktcapfkza8d97CrSAiEAs%2BhHTlcUwgAAchdFDLoT8cDJ7TgTUhFcEshuNWtnbZwqiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJaC8FjoMMSYR9dDQCrcA70wNTNnrIMCk2nHHMiYoDLyKQ9JfJnIz%2BmzwQzg6M%2BkjEZDGlrgVH%2B8Y9xG6FJHEBK75h4WJZVWtTCL1V3TV1Dz8ItMhcsLl8ooyCVRRgSWu86PDiVa7t1QOR8UYHGhfcOJ0fyUj72FdzNNP0Hyy7cnTJfRHfh4iM4VmrELlDXK8YrWckg3yCbqkEoWWXGqJjABpdAAlBFnZtQTsy02xYkSMPw%2BJlY5gtj3IAYXzCCJ8mYxNuQnHmob2H9wcpuQrviMRuc%2F3d4PPAQilGjrJUoJ3UgnzcntW3FctmmRCaLW6PVz9RAjvKqhZ9TnD5t7fHGdO2z4Ph8Txzf3r4Uvy646ZM7wwOnKV0boBfn0bLTPA2vvpKbAuERhybrhRyyIxQM9fvn0xZHFch4qTaoEWo6BI3tY5jgyaODGVXAJoLF%2B0Y1M0vA8gGnXKrMNNxsAD89YAUMaK04kVTSQLq8xvX0slz9wBVr6qF7caGXWboanIT2naK%2FduEB432pUIkyIJdaO5%2F679iDB5Pf8Dtou6%2Bi1I88eW31JS3Q1gsfmDAuxcel%2FNB%2B9vssszvtYbtsRcRaVwfgbaGDTLNQ%2FTfSy60aUIhYKZyIIvb2h858KBhzptH7jzP7QpcGdPvzSMKyZrcYGOqUBt1wqL%2Fj4V1A%2Fh%2BrTz0yzdz9mqPWtY1ajGpY5unJMKIg3LlxYuTVKHFi60O9JQxhcudtyy0IYz5Jfdjf0LE%2BhZEvO5B8FPYjojzt0is5hFZG4UD%2FLTVN5DYB6klT%2BWqOM1ZsxohPH3MeqlN25YZBqIyGL6ec67Mo%2FAcablQ5h2Ndve8QXVWPUyqoAldP0J8UGj6%2FcObmCgTJ4NlWOZq8oInBgOP%2F9&X-Amz-Signature=39788c87a7e48af0f2ab758ab11952b64a70d08fca6bd3ecff12c90ae519b707&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

