---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJ2IYCSN%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCVKxs0r8hC0ZCukrErO8fVwd0mzGF%2FN3ar%2FnwYveJ7tQIgQlGcwN4X8Fi2jic%2BWcQ2eNW1fxhnlsDPfQtIM7Rdl0gq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDERpWExtJ0bYH8XxlircAyQdjq%2FZ8OXsSIYhyjy0S9K%2BZ69dIoUW%2BRvQt1Slw27Sl7RhOL6GxPXYLiETjX64ixHnVoiBr5aw%2Be25G%2BtGmXST7m6D%2BRGlgct2mxrrFOkINtI5lWdyniOdLfqVi60Rbupkbi1LVguz5EzeY9hBzNFdFv%2B1xZLY4R%2Fv53cGw5vbWP%2FNYGEhbX26JQX33%2BgTOWrx9CCTi39DYgVM9%2BjDqcpDgIdsKQQDRqDTxA5aD6nJcnEFaVUphyQAN7Hmcl6B255s93q37ETItrEPTkuXy1GvdrVli4sKi4xSw3QobBLqDJ3m%2BOLEuUjcyoftzd1sQzaLCrBaqGq2kB1j5bV233ozmkfod8UH31F3wiss9OnodtKSf2im767qsJz1ruayJm%2FjeL9Z0YVJlUKa5dTN6QC2uFE5IsZPaQ5Jl5eo%2F98prNM4fSmhTeSe%2FaotZr%2FT6JmErm88sDMo8i7oLVB58gS2qB5taUrCJR4ca4JiGBf6HIfx6TU3sYE15jkcf7mrMf9F0G67wDO5ChPSGWAeNMrlys9Cgft2rsw7kv51Odwv34RkumWVMfYhXzt%2BH6V1CeDE7BRhGGyJHVFipZVQ3vgGIO8n2%2Fro9jmPeYVbgFGiDa7p%2BC6bX9fwQJXQMJ700MgGOqUBfEDh8t6kTBcwZlYID%2FhvVFYA8Tgz2Qc3dFNcBw%2F7BAddl%2B3ReQ49eydsLOJbUoKE1kGks3IV%2FCR1JkG4peK3%2FdLydH4lie%2B4bSMLc3HD6jSHVwWyI1uiwscO%2FFon40ejiOYfnoeHUQCzuXPqKl7vQNGJFp7bgWMIbNfmqH3jZCOIq7w1Heoc51i%2BzkjWwk7V%2BP8aColYo3h0%2BykzFEyUik69WuQy&X-Amz-Signature=c5afead7561acafa458201810807f826bf7c11a6d156a2109cb03f2405d93073&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

