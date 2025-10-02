---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAODBNLR%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAgYJEJlt2x5IXl0JHWteCrdmnT9Lfh9iRlpIqj83LuQAiEA%2Bnrci0F%2BXAOuZ1e%2BD3Wjl1gs58AD5bDWHoUDW2T0tm4q%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDHPtf4Xn%2FBuFBxgfwircA%2BVYk4VAHeMgzoKaBEIJmjIh7%2FeygK5SSWr9OVqxBPhuQtFidGGV348tSKxA5TaMIWM7s9ZfZYwC%2BEUHkJaNF20hQUO5D5qeHkWX4zbIrKOlBY2VH9j5VsPtDva2mhgOYPkshKihZlOTBvpVvJqlhTbM9PngeOaJ9LykNmbVL9vqyi2DhJHv9rRx4ybNuAiFMXqNBG17jTvtkWFqohNzkbdYp1hhpZh2A2%2F22%2FEKXoEeaTXhqNsV0BnyD%2B4QO7BCevNWDGcMvPX2XACfzfpNoqSf%2BvXvktzmvyulKyjoO1v%2BVAx0PMhJTZ3R9r3kv4TfgUrddexSNc41Ca6i9RrKdPWXkLdziUdragMwwZGLSCnkVYn6yFuCn8tOUqrT6Etl432MEHuCx7T6C%2FhNcHEBKkL47MG573cGUFFXpIIHRwpNFtB9VuobJBz0pkGw9LgP8bbviGz2pmc9zYE5RmR3b7thAKVra46HTJVb02FZX%2B9i2tCd%2FYg65ZYTjFehaVWuzmrWyxVVmMnPOfflaDaotXa%2FFqmhZVpf0lFFK96M60BwKqiE6Duhc7GlDUyOPhzkqHZogNx14RDxkZu8SObH25DOpr77oXf28MzWRHk5yeemISoKKJ%2FVAWCM67S0MIOy%2B8YGOqUBZds6d6akE447NDmGLetn3ygQnptK%2FN8FHiEJGlsCWMY8j%2FoJRGM3dOfMMp%2BBbfqQZLhKuvr%2FkFHWjF%2BOnpWgKLlbPANnMKPqVnwsH4ujk1eVdo%2BagwpaUo%2Bbn0xku4mEz9v1TJOLfHgngf7M9DWCDmpRdv1E47PlF6lwMZQcJ3%2FNCsXY53wVeIPBry%2F%2FQCH5G%2Buz9kuUEdAZgXsuKY94NIX2qbHJ&X-Amz-Signature=d8312e0c550539173782e0c33498a19fcccbd05a9e69af5047b488b15e9415af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

