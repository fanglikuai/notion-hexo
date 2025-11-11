---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OR3FOPW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQDqZ4DFg49ql32yBXfMZBLb2ERkQdppgaTfQGVhSj%2FuwAIgNjeJWmrPAFJrOEIJ%2F675wXwO%2BBtE7rggaaHoqH1ajogq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDJ7E%2FaBC4h%2FB%2BRXIOyrcA378avRf5akymzWryEk31vj3kvbc8dCc%2BjS51qRN2TCvUDICaGFoc94Da9zFA%2BKe0jafvI%2FLsUZmGfC9tUGOHovxScKzejIXR9%2BLPQhRJ%2B1CHaZlXzF5HwV5EzZ1G01p%2FbSo4ezh7bOfNkoZES3uKZZ594T3wK%2Bi%2F36IMFsvHKZWWucogH0mJLUQkwhiGPjj%2BIdA4WrerfasxGMyYRHcbuDStITkfD8MmD19tlMO8IwF1SgsLQ%2F6If6gwnwquBFXXFkPmx1flOH62Bc85YczmtN2vXPZRXzv%2BISao3RH%2BoIC9vE9MtffGxmKp%2BsSd%2FG64Qb3bCES31ZrDPyKb4pfr6D59Jyw%2FYlcr5%2BU4m5xWljNrfOEDwgm1k%2BKUP9MHNJeC%2FhNoLEF68hEVBoptHg0rCI3ym%2FWZcw%2FUAiMgOm9N1lWidkY9M2VWl9au%2FpI0q0O1jdMV9GPktSRMFhmecqO%2BLcUg9I8DtR8oMsulfYfpydnozYu26YI2BvbH7bds6COO0r10hYh%2F5aAAxmCvvX%2FTmKZS94ZhflwmnbwzlDSgrwhvp7SqogsD2nPXiJUoN7NUozmfvVt45IeXzbshJ7OWB0ljrNc04AFrcxMcpe8VVFMCfA%2FnBvBYBJcBH9iMITpzsgGOqUBx4%2BFGIk4%2FtKj7RVEMHHjJtNGDokjIVFmKM2M%2FFe38Df80XyVlasP8v7f1P%2BMEOAEzZLY47PTh7CqD1kxcCVCfCEWFf%2Br%2B7oh3%2BAlQ6Y6wRATZSOiR%2FarIkuZ%2BrwERwKfZiKp7%2Fq9VvzVt%2BVnE97fHyPA7XbcKu4doVWtBCJLH3Jc1hYC6t%2Br9Joc3%2FsZCX1BTIp%2B0m1EJ9x5g%2FheKG0YLOZRvAJ0&X-Amz-Signature=7abfc39103891bb2144a38c63681dfaec9c7b1a4b5230b5c545f21553f87eeff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

