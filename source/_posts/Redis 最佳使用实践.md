---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI6LMTX2%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGcNfWSZHH5ihrC6PHAxUX2pgu75vlI0CwKe%2BbmImcIwIgMQaJlXt5EGf4Pdqheiv53Od02zJ%2BHjk1M4tbD9VD4ecqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK797oZhWBWCDoYXECrcAzxRXQDuo2Er6QNdQxMe8cA2tqIafZ4pqI6ox958dLYYmVKOJkYy3cGmfh0uXSBwqbErOqp%2BwKilwHg2tDvhjwsO7nmqx0HlQIV5yvp5PIJF93WEk8DGjpmODu9OZwZpaJEcBZySmEEZfC7Jgl6cVvLNEDD1ShLmvkSGPgR1vGggiExFJJ95RxLiwSN4jWED36JoY1xKWZt3coziwT9Wrl1EGAu9K2aTcH9Fag%2B1S9J916LVOSU2b28brYKyh2J7A%2B61VH5wSV4VVleDkN3A99rAbEeF4LuWEbX2IUM2CO47dlbw90W0LEB4O8Uew1yxIT16Y0Sf9FBhy4B%2FJFrs0%2BR46Wl7Y78xcEOi3fURO%2FKNrnbDACLbrSXHiHc646zFtyCj%2B1fvYUGoLyqENG%2FnXm%2F3r8eCuDpRLZRyAc01%2B5OkGcwqYR50%2BbYxDvlNNqm2zYXlikqGEoDFqn7dhn00tYUfLWI7MaFcBywjcDVfeXS6iMpgxWa8yngoj6AZ6KtW6NBQaQTzV9l%2BNHuWKiIOSse8%2F9QbPTMXcw9B6OY4%2BzOrK3wG3W4ck2UHOl5tdP24zADvxTBKckQgVq3Vi40%2FNZ2xW0Vf4%2FsTBTrFb2CC7dJjzY8dsrIRpnJdpBVzMIrP5MgGOqUBEsGGIfesnEHo7kZVEICL%2BmSNNF%2BKGe8x9NxlQ7DUCWT6RygNpESO4OXDyAWjXrc%2FFMaVSZkzPOCor5bfFXcAW2EZsxQBucrGW76aXcPfbl8ziGdnyo%2Fr4Nm9JNm1%2FagdeLmbub83847hJ15ospCCB4VpHUrlubIhBMBG6ijg7EdO6r5jyO6gWneMVOilu3IkxbLLTpPNNqba78qUgyjEYgr3TKme&X-Amz-Signature=f6be0afa112e91f194377efae5b866d3c2739a36632dcfe29169675a3e9944cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

