---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6UA27HN%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T230054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICdbc489tu1lbR8hNdeek5xNMffnLeTYUp9nzFdvTz3TAiEA70GspxJ02NjnSEM5uk5qYi91FGh4hWcpUIRhXaCST0AqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQNfuqDicn%2BoKkhjCrcA62Z8uSBAzIsQ8gw%2F5a3kYqPCZ09Ho03X6xxQnBMAttKWlRiRMn0WaPs%2FwM7sLCgsfvR3NNbDnvqJ0NzW1e9dl1ZTVcyAHhK5Vb6ymYkpgt3u3bpiDD%2BM3fM%2Br7a6cFcpIiJAgYfi2RWEV30Vm0GywUbVq09Gd4%2BZsiMTt5lcMBurk%2BHnE66lgR137qBQ604io2Da70U1rXkgi3m2GcLUrLKMUlt80XWel3RQYtav6cE%2B70Sd6MofB%2BfxTf0QLoJlD9ymKNQi%2BOpNvfS7i%2BtZL%2BNGdQh75CzaXc3wDCMav3mIYUSe%2FBOADWjU6pLoO8pWUrd1YR46XDWRjuw6UbfWrKvE51mQfHegyD2g%2FDg6I%2FH6nn7U3zibjbFhrySbibEdZj8lE%2Fj0RZeT9v6KQ3VrNaD06pU50ALyPrPsLzkjoREfmX503nXZPvEKX%2BqA3%2BUBAM76yShV2LN5XD%2BDcYVPgeUb2H0u2dmEgxjW0Q41rXAnKt%2FSXgbImMBv0VPAUIF0%2FWTZuuvCEGRV2tlTb3KLCs%2FQKdynffRnnJGKuQZuUDsi%2FCa6O0G0X%2BjodMtEW3cDjH74opnZ4yexZvrYHtqkpQa2dq7putqZnxct3OcQw7W%2BvC114xy0NaEr9aXMNOR6cgGOqUBcrT8oE7cI3qe9gVsYaebYj1sro4hjQ2qh9st5ZlCc4T94t6OnRzlE3XFI0giLUW%2Fw7Sli7LPC0u0nS04JxIhMdddm5hhu%2FysqYwza9ho9wYrsw2UlkTPIj0rNk%2B%2BVb794oooe73vPKbZ5H5OGHCTm8XtI4IcC4kYdII6A3OB78sW%2F7jIwXX%2BYJUsccNjtTZqGaK%2BFJXncxiTItVPzXJ4O97cXfAF&X-Amz-Signature=a37afcbabec4e3812d7a292a2af2af321f11523f6d9703f1c1a6f19969ea4755&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

