---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653EQ4NUI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJHMEUCIDNCl1cgBQcczrZsQ%2F1D3MOrBSYMGQDjVV5OK4lnsNqXAiEA5iwqpyBmLvPaQSHjqmZrlvI6VJU9dxv%2B%2Fo9wtwZheOwq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAcaMlIUc2oVeMBInCrcAy9uZKDqCpfRgaeVTBybyXnD0DxwKZPphMZ90ATO96bf0wh1LOkaTlIDPXTMsJb62k5qEna64VdUq4uUBODA%2FW2Uhu310%2FjhL5o7OrhF%2BeSDSZsxUTAbKPskJze5QdSeVprhINFa8zyoJ40ZsSSE0dNcpx84WVNmmjuMxaPuZq9iUJzqqePr539UuPOk6uCGEOsY6ZuwY2JYhE26RrQ2xSWp%2FIpAgA9QA%2FPwebHIrL%2BDiiHFMSmOXkfh%2F18ZsRKBMWbyKalOS2Xki%2FVZ2YNwR%2BHCa8nRrOdFWAaPi1gpDQyXf9V6boVWN%2FSZZcFZv3IcPr1g4zng3W3XDrKUB972zHWV85D5Gfdi9yLvJPEBkdFmM5hH0%2Bkvq6SYx8oA8p5amWLXXbEPWKirWYj63DseMOnHOMEVTW%2Bv4PDYvp0eC1xugUSy5QZYzC8iyEoGKBKaVv5jhXSBwgk6lsLEGpAYuHqX2nCeZ7P4I128BuhB7vm3w0nsa39QntPgVyDuKJTpmhNq%2BrbWGNqCGy9qeZzJk0qTSSopVzAHfJ35QKPU4IEyvXkaCT1j9m6jbS9z3BsMcJcQHKstV71ZtDea3dDp5HIH6nzlWmcygwOODOD1WKvmDVQnq8wBUFW1K730MM%2BW5McGOqUBxKe5WcBjmNMf6EZXHHsjxJhb8bnF24lyI7mMKb7MJzkF5Y0SvDJ5a8FZkXZffvouqimGzpt925%2BbwWzlayI75g2m5GZYQebpCHI9EmVpzn0IKGW1Rv%2Be49G%2FJgYBg6JgEPw01fIPnBh%2BH4%2FNFXBWjxotrlsGz2Y3dak7B1KEm1K7AFJBujLCYawIdHDhUOGiYM3lTvqA3IGzbjlkByv1hECJCjMM&X-Amz-Signature=1dfbbebd5e591059634857775ed1991f16c2a3d89ae8ed5c2c8b4539519886be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

