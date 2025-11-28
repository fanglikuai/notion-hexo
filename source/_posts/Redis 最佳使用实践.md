---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FUTKEKB%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCiDmAmcmZEpd8lClem5QUO9gGQRwqMZiaWl0vT8DzPtwIgCHQfM1msf2dI3vngPHm8pAVH1JaMfI9SJxMQJyD1FD4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBttHyQbQ4VnsETWjyrcA6Jy33L6y7YNhz%2FquOvuxMSakykwX%2FbGB1pwwq6w8sWgh2uvJoJ2jMcFFx00BUjFyr8dh%2BsLBj8695ZE2EXz1l8mQftO6sbnmdyrfeN0jFcyHOqzWwb9BLxXxVvuX1Y6DbFwyruHuSh465%2BStHd452ooDavCPrmub6SrWhRZQ8Vb6%2Fz66vJMIWyMkiKntp1wSCg4MY4XvWxB0Fb01CJBbQEJNHxUkLHWDyMEnFspPTpbx8keY10CeP6tp8A09dZHYH4TdUoJCRK5tTkgE9jTVVnh%2Bughb4MXupQNbIoKkuSVrbXjA45kTgpX2bbeglZ1WLYxD6WnZ7KikUHujmjXDDN3rtE0aNyet%2FaDCST9xsRUGwN0yHyxphJz2SGCIaRz1jXM7UTjIZ%2BQEkY1A%2F6dzPfNC10SsJgWJfKGRBgzFxP%2Fq%2Fr3dXXLt9pGaXCg6izso0XZqsSRt8Z4xXHZdtpx%2BWw7%2Ff6YttYyocG4eGS8iygHDDrenxfx7VYub83bt4T5PHLmDg2s6TqqwzUWMPM4Bg7E82uUFbwQtOCDcAbh%2FJCNnx%2BduYxFYBvCFbjqPwFoLLO2r260Ijl7UWzVNDpvhdqxxdC5O6vgbZTRSnd8wNEYkx53HLcOTz04COQDMKKDo8kGOqUBTofNeuCZW1cdWvgZS0UL7VAnPszp6dg%2BBuMa1E1IP3lDQ3%2F8WL7jLyjKdkTM1yWWeIe2nld3CtDXJtWitffuy3UW7DAdkrw0yOWKzJ4oSTj4Ot0Rhe121QllnfV1AvQ8sSpTf1TlVlmh5iCvQT%2FdvOF94zr%2By89RnqkTeqqx7yoOQE5y5nT3F5Wj%2F74N0402hPbumO2Z2spIw3RoEPskCa0RNNN1&X-Amz-Signature=e6e4b1a805b3967e9ad127bd64f7d6b38d951365bb8b771e0620d4ec17966045&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

