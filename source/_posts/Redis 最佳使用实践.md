---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTR4YFJ2%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCpLyL1%2FV0iUD03ftgSvYCiweuLh0WUN1RiLvM4yH2pawIgHpNN4lWKXobcacB4zOWddtTTBZUceXu4%2FTDY0aaYxcMqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKbbJe8zykNIIDnboSrcA7p7cMEz3kbBAT1duCby3BIsWH3v%2BbMrsJjEwgLd%2BSJQuQ%2BksVQHFWT6zFVwUbSBIY9RbNqz7KratHpKL4%2BAGj5gEO%2B0Q%2FsXzuF4%2BA2mkBnD%2BQJsH47u3c88hcmWab7PXfDXzZUwPFyFPoglBHxG0CIUe5XqMzEI5tvtDGMk%2FD6d7o2y0Ao1atM3%2BotI4jY98JVfAuFeyP43s7ZmFi9KUT9%2FAXzVZ%2BNY1yXREj0k%2BC6VQvOvZKr4x5OdzYXPnJ%2FsDb%2BqYHO5jl3SD6P8NqBeV1luwXeIwZEf0bwt2e4y60dQ%2FI1qE0G20OZrcecx0GbsQS9s436tc36unifXCkZqGO7%2FxFlQkM8Wwi%2FehJvph7qFKMI8nwsMm%2F5u8otTTY7HxrmpvJCmKro2K1BUqmMIl%2B1DoOar3a61%2BtYWf9M1LWo5%2Fb%2BlYeAQIKhXpEHqVsfc14noU%2FZcYNbCx1pB3Tf7CuaaDBmnaEtdBNlJOw8J5iO6me7N301AwlTU2XxTHXS9zAukrhG0S93nvrv2peHJHO3O6D4sBdqMHTrLMQBzRrvfgFyXZ4%2FoObTA%2FGCFa1LSf9wa96RDB%2FRSvybTKhSAxzjMdCJ2KEs4aMIIhy7il193TGZ491Ca00MWe3JPMOSdpccGOqUBHNFyt1lg%2FP2gp5tBIVB688yQ9nAoxZEBeYO2BW7Abe2bK7tttARsmgfmKFbatlEXF3o4zupHUuaNPFlGM%2F%2BD0%2BO0aKFSC7XSBoitkMigIHSqsc6P3EDVJ%2BQc3can6OuOGUQbRHNyiVFRda7crjYZOMTnhKqOEdv7UR%2BUPwYV9IEnbYqjp3Vmx6x8Hj8gAWF%2Bmx2uQwQkFMGNbUa%2FWMAjSazD7ai4&X-Amz-Signature=aa4601f743b9e5dc5e0e8af40dcc2b3b3203324a49089beda9c353c8c58b5b37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

