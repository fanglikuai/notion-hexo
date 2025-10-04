---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSIUNAOG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZxLOcBegfPJMpgeof72Fkas52stQgMoY7znJ0GKbxIAiEAoE2qrUap9%2Bm4MxaX7vfatuC%2FQwW%2BFk5uVyfphMHh3b0q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDOWiCngWLk9zlCu21SrcAzZIb7ZdylPQune4R3qPyBztXupndzxQoa%2FIj7HQSM3fZwzqm4HbwnjFbXPOK1rtE%2FDhGF7PTQ04V4m%2BNlXX93xU9ygyb9ZDOmhBZBycD5TCwd%2BTkY0USVr3rhNp6pgHQfrxuvNh6yVZGgLzo%2FPcr5fvoHqIhRJ2vq0oEZkIzUxj1vgWCVqetmGXE8bWftSyv3IeNu%2B7D85wdHuDp2jrns47KrmDmLJOJHKv9gfaX%2Bg%2FncXr%2B87CVXV1m7QhXx3RYF5XDJHzegRnANXpu8KjsGgWL6gmhk517Z78aCmZ1GK2pmsE8erHFZkWUyMiXMMo%2BVunYx%2Fu%2F0q%2BifLXAEOtdKZKH4IRRHdBa17O9L3Z%2BjBktCVi6Iq0WH%2FIA%2FFnITaMY%2BG0QJOIORmc1272QWHjnYDuxWicPiqKZmEM9Bl1XSeJsXVPkFCB1F7jjCaIBmTLEZyC%2FujdtGuqkTYjm%2BhsJtc7gADk1M73x1DPDub95pXKQpD812esfKDTh5seCR1BgxaSuUWMayPAVmg4mkDcksj94k6ruSYPQSbs%2F1SqFemuWaiHxkT5quHWoTKSFqwo0RA6eDYLw%2Fg5kaPgVDLpbB%2F028JzHdcRHo2aevtMll0jAxaUW6zvDgGFy3uXMNKbg8cGOqUB2dR9WgpWsYaFeMCDNXtJLrb25DLyz7tzJd8Gs5n4MZVIgri7bI44Vhgce9syhjnvpNMvgKKsqu9gkMAk6Q2tAue0qJ6tyZzAVZu95Zy5u8yPCdY%2F7BHf25mu14HL6liZTyO7fuHZhQYf6GWw2UBoKf%2FH45C4T097jXwur4BHvLakMfnPQ5J1MrKcHQRdhcYsb8Mzp2Qi0MMagUI3Iwycce2k4BKG&X-Amz-Signature=9b3a37e72b9dc069212bac71da11e9fecfed79058758ca30a5332510debf60c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

