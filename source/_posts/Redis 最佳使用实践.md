---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYZZBE2U%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDorLTKu8oMQeN3LDhfA79K5IugEN9gnG7Y7v4qEmnvhAIgNrcF06huSbHB%2FboIfMSs%2FN3zj%2BauapWfLh3b5Rh5IsAqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOKplGqnq%2FT0oYsO3yrcA%2BrXnEDl5l%2BDXQLmwQrcwBNA0xQSuSIGWzGaAaWHiThCJOi1ypYzP7cQutJlCLe%2BqKLkDkG%2BaYpmojqGhEqBBIc2j19a4G4tkTC7nyiu8FT6q6jCYJdid5Ow5UawtxGwZUy%2BlHJ%2BwzTlOfczgvHyaEAZEUKMAOI3Zpx%2F1jVZ1ov5bV8a3UEMLh8xj1IdGqdc1Bmpug1qcA2X2e9HrZ5NybrogrWbXL8Ykz1hdGpFzPOAh8AGbJexukP%2BBQN58W4xajrLMrvQctwaqMDYL48p4vRBWqaMkK%2BTTbe3GvK9hvrDeBo44pnqddWD9ukLGe4ncmyPx6S1nOXntp2ZnmUWml3iLVYq5W66O8OD%2B5RucAOqmNY1THAHLdV0qrPiNzZXIxaaUHc3wZP8low3PRwT7cy7tAC2hc%2FZvZoIypfZYCl3pmkye5ecG1X836I9v%2BceRPy5YwrnQvL6sZ7MdrhfC70eX4x%2F3NzUXsxhj0Mx%2BTUdG%2FRo37FBu54xT%2F%2B5FdpnURNhtExGDJ9c5OsYykiU7lhKw1wXXFKfOiyTZ9MvxzCfehbKVJIvx4pkFDWDrUk8caojOVUZXPjYpRQ73r9KGOI09rMJCnDT9tTue9t0RTzkWtMvXOz5nz%2FsBls8MLWgssYGOqUBbzAcpgexknIerQs70q0qURoRqc%2F0Id8yMUSQFGbboIH38%2FylUwtj9uSJGiGpPF4ziv27cZIWBmGEJy75w7UQSV1fM44ca7W0QbbQSHHuZW200%2F%2Bb%2BKAXf7NPWwDV7y6LC24uxs8prA8Bc8J%2F5FDxXVlap6hhyEegUrQpJ%2FVmVZbhXzmp5Wwo1B4%2FibDDTngmOoWkxswOLXHbCsKm%2BYlVCFehRxa%2F&X-Amz-Signature=de9426662e1528b8fbf92e08aea0658018cc07a4acdf1ecba4a60e595eb46653&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

