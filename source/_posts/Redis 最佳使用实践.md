---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y4BG73BG%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDEIPDxYdjBzDYf93c0DTunpdySXchD9KfZNs6kFlIuEgIgagwa5U9zaaYUEgEACzsG78x6pZn0UjpqDQD3WeahkEgq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDL5chW7FiPeejZ151SrcA4zWSYzRAC9A5dACtbG4%2FNHE9ELtkx%2BJH3Q0ntv8WKcgRt4MCS6dSOkt8yu74UY7KsMQPK7dq9kq%2BCySTcjmmJK8d9hgjeM9YPWyR4JS6t77J23tHM1ZFZwcFJuO%2F2EAvned9PMsqmESvGFJq0G7XBsCk9ZYmWV3Em0jVbE7Hh87yNtx6gj6WSc%2FGES%2B0MCSl%2FOZfjECbObZL6Vu2%2FVyiEvnbiF5e4LeA%2FOX8XNi6EyxiwXQlOItZTyLCPsmV%2F8oY6MDAg0pFGKZ%2FsG0xxMTWf7jNfbgkZJy8mw3GftHImNcmdC8JHvTpJ5unaMgDB5KZuCbQZlqAA%2BePhtGgm5agqIg59cH5DEty%2FZVtEo5EZi%2BhsS0UMalaps3ZWWbCDG0OHhz7mb%2FFLw4RhxTxcmiS%2FMow%2FX24Z%2FZwtATNayNtKTzyrbPeY8oMf%2BcWO4ZBCphESSAApbbIB0qpnybhOuEbRMkr6sUt%2FSU6e5DQ82PsgHfQoX2ScusniUMhy333kwVwzqlDZHtXD5abg%2F4FJVZlODzSsgl7nLSyDdEBMql3%2Br4qd9xO4UVNmk%2FuJz69NrsvU70SbkLx3TvF%2F76XxnTWsUCs%2FRPOUG18fgX8Ia3GpYHJW6dvxARx%2FrQ4H4xMMf85ccGOqUBSYgSTODlb%2BEhs865vDLtaYBuiLWHir0i4SO6ztMC4ku13OIFSRgKua2VSFp69Kp2qAUf%2B2WYHfdA7%2BCiQZ%2Be2B5kd7yqKqvfq1pdI8MLPPpylKRSuakzNV93SG6C59PjWjh2SS1BEAgKJ6ysGLLaUu0vjVt3IyfpeXtuKcvV9h7ovN6i6hnturiu%2FjcT1NZKfy2J%2F%2BmJLKikY%2FnQd%2BAI6a0gvflm&X-Amz-Signature=3f3c0c24b9f0607a11360bfca7f41b028240be45d35152016e09c055ba27d0f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

