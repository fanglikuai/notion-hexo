---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSFEON32%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T010059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIEiVEbIJ7uBxEG0oFmqaLje38UdN9QTF8%2FBzqnCWTfsxAiEAjhW6n6Dw0FmRR5cczpJgdHfrsu%2FMrzwzo4xPlKqiS2gqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAnGuMumpixMFRz2GSrcAzGgYKSOsOLMbWgtk4iOPeulDV%2F814Jgz2xVFx8Zwn6jAmR8NSX0LFrr5iKytpTpotn%2FKRlis7E21%2FFkd6WB3BkgPVDfFRxRu30b6da5o0pzOsbTWxUoaL9%2B9xks8ePO51RH%2BRaiunTtm8saiU0Yp2h9gjC26xHPtEUFuc6SIymcwkYFbRAyTiYf8uesixMwjZRs0YZPL2KWRBp%2Fh8qeLFOg%2FCdX8WGk9yYyBwLAJxGAhIyQ4%2FQDWxXHJY9IWz9p2RFMI7sLWRU2ei8QDoy3UiT3OB6%2BMOXUAlzQ9rPk4DkC6FZQxmC1d4obyRqvoeuTJYSjaL2GN2qd4yfLd36xqs9yNulq1THxC0nbfnNbnznfHsPnq9W3pwqZkH3XpD90ArErs4O1Mdb2lG3tAsHjeLcb5mVbK5LGkshSW1ZSMRSEUSV7%2FemjZSSL0Gy%2Fjj3c5B0uyEW1Zhdqf9KcUHfKwyrlR4DCxkGxuvys8fm%2Bi4pdeusTfP5m%2BaTIJASzGCUKpxoo922nA8s7gFW%2Fr4G4%2Bt7vT11UaJMRjAN9bCHV9M7UuQOcUuJEVVoqkuTvEmwoYv6dSaGBVSYn8rvSHCuQjYZnkkNXNQnHdI5YlWKplRoxEs5PHcpEdiKkfA55MJCW28cGOqUBF7YhdDepcLwT8D%2B5CtFC9NmmvI5bBgLjGBsEp4n62hQLnkY008r272%2BXiT8dvfT5kRkndMTZHoSN3atsV0SCm42n1uafcaI0OnENwvbnU67MnB9Qw5wm%2BD0qkvuxcSnW%2B7sr8blZGPjQauRyh0Ryq0hW4Yf1zApM5GeUqoIKLtzk6kqy6tpwlt30X%2B6UD9BDyczWD5eQ5L0BQstE72MPkuLP9zeA&X-Amz-Signature=b41ffe965b77426e71e17a73b4470517e0303d976110a32701e991a4d7530f61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

