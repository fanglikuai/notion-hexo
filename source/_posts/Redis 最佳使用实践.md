---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNF6T3TQ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIQCuBm9aq%2Blx1Qx7vWnR7sYaHpxBH8%2Fl5Yl0gWi0yLvG5QIgKAZMbGgG4Ko1JN5xF8byTpt5xI0PB1D96zXdAzFCJ2kqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAaSIFh%2FnWNnbqeSryrcA23yQDsmNhqHIK%2BmZo7oNfdrIpD42pW%2B9EJ%2FDXT6AoxH3tEDR3y4%2F8IO4xSBWLumJlEDap8cAhLq%2FfR6zwvyroNOYbiBZfiMhiP4cmaV041iqSzhMOhQRKq5IjospEFY7MA2HXE7dHL84WeKItekjwCF7dvPB%2Bh59fUcdlRkvd3dQyzkwop30Fwr1df6zTUj02euhL7lJ73cceHnFdLs%2BeiwKX2GpTxe8Bpy%2BtBz70C0Wa9x72ShHu5KEo3Bh1V6UVd9c4KCwmg6jG8HdSNoXZMgS%2FfYpT0vdGTlgm462rpdfxvT9oWAv6ddPSnpnz%2Bq5jP9Dk9a%2BZe%2FIStN%2BrjN3o2bizOuZVx5tofsTIn4q2x0bf3CCyt%2BhtkcOQGeMLYGIfocUMCSU20qjbbPA1r2YOs1Y6xJOfo5w%2FXvQfAv9Rr1ihuB2JxUsbAdH%2FmKng1XNQwCdVu%2BhIHJ%2F061vSYxf0wNhMKm19jMNHSuqmgZ7u%2FNIX5bA8m74OV1dt3DA%2F2prr1ptB44n8j9VWTipwbQiLeWk0Eu5uy8%2FIBg6JyjUwX0S2Gr5ffTb85%2Fctg2%2FvMPz4avEQSf8tKQB29ahDixxtLJQIA%2B5UJRdC%2B2cXB%2FAXllCd8K6Rz0Qn%2BGqhpDMOKyoccGOqUBDnI6zsKonwFAct2LjsmnkCWLggNSEZ7m05a0Vaxshh4wd6Do7arlxTnepIyidGYykhFgCpz0xJegaXKjc55lnbkgwHVM01H68Uch85fo8ldQurmvs0pYRJz8OGU%2FCHYmG1iOiOLu89DCsAd6T%2F3DFh7nPxAP7lgE5CPcWWT5cwksaFm1wpCqXWdW%2FgLCqLFmUG4p84ZCLTGHlHovedM9RDSkQ8KR&X-Amz-Signature=90ec4d3ff8ce2d2d4c5dc044304528ba3d5c84d94f6ed06fdc4a1a66e6b6febf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

