---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVIY3SXV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQCqyi34dchLgUs2t2tQKsdJs0k%2FR93YjnVhOQIWsnCmEAIgc7QzoS4G2vjzGRcJTVMmIG%2F2D9xFXLJwbuYOZ9kgqH4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDANEN5Up3q9D4SM%2FVircAy3OxsuM3agtrHYeomRQObk2aasqx%2B3Z8SUPKux0PxajscL%2FGkRfZbkCb6DYpp4s20aHOUYj6ZuIXPVsxxl4asXkAy1gCVVa%2BeXijt7nYFEjPVEeQU5El4CEjw%2B6oyiO%2B%2Fa0p7fRr0eGo7cyIL9kTKBmmg2sBYuCLb09PvZnmqskiL48VFUJ74qLBw8i5Vxzh0H6lcc%2BLhM%2B9WwMiEzL0y30MuqS8B0y1nd%2Brd7lG%2Bzfalh0GzE2ugVp3P%2BuzQbRTsg%2BRjYG%2FLBw1uwm0AzEGq0ilNfCGSEaWJnVyZlhv1Ajyxh1DASTep5wuzEiU7n2R%2BJTo8hRUkbtAN%2FSz5nbmgtvO1iOP89acxUmwHd5YcYDR8zdkty7NmmHGEtcLGTVocPGqRjPI%2FCUeJ21FFrwsJRIbvR70KkGyvg4zVdaBk9xlBC0BitNM6xdUm5eeXs%2FikQGHuiD%2FBL8yRd%2FbjqtDos7oimglqYkyPAQANCvWf1k4M%2FkubfLlcORJHCyK32aN2uFxia1PSEUCxCo%2Fd1aC0RPqH9RpbquELL6CMaoigRJhAfX4NjHCdfV2sC9WF%2BHUOFM775VwL6KA5F9gGQHXjQaFtLPU3YtCRzIlK7E%2FKh3j87GtPVRAentsvGTML7nuMYGOqUBANtsNoGxFFcaOX9sTu3WGt%2BA%2F7BXxtkwgbu24Um19NkMZWo1SQ6zNJfhHi%2F3LBBeEBPRfzdJfdP0W3wkbF9R%2FBZRgM8zS21LwedzgEJ4w5VIi92%2FusegfsSpw82v9FGvJmTMzKWMImjJIPn3S%2BM4Wnei1UycYXzPcGeafBEbkH2S0%2BugrEFuF22wABqm3ugyO2AB3C0Y1CyvoMjQFWv1eN8LYsau&X-Amz-Signature=497b3a2964a93fd035b818c29d78bb3c27d444310bdb6c335beb4df8328bef1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

