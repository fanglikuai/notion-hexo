---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SIIDKU7Q%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIHC0gg%2B3R%2BXLd3VCskeS7DJRZ%2FXNt1z8fnRBVvpm52zuAiEA8sys6zj8B0YSI3qlj6RBVDp%2FfI6qiaqFLPB5FktOYZUq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDCzdGcZPqoVaXylWuSrcAy89AQ4%2FJWd8xc6cRp8UyrpsxBVmftKFzXzjNlbbxEhNRcXQxsaKVzByz6ofT8sVhbIUkVbY%2BXtVBbMHJ94KMhnlw4V1WiBrSv%2FUHJgZ4RW0ckE1FCzXCaADndxQItavTNftKWa20M7xFH0qFygSzbfa9HL3q9VIgPDgLvBuw9WhoiflsCVYwXGJFC6PY%2Bb04WK7lwReWDhJex5sTSrZFePm4%2BWJtpLJYtA0AjrHaRVgUoaIZGcLg2GxTVB%2Fy1CwRyGf7cozLf4MsKEuADY4ko57B6G6pIhiT55TZcOzcPgBmuI1n34GvfZPA5CZ76PgyW9ohQGgm4Zn9A8rvgeyY3Ml4qNi1f%2B2Hb7TUllWGKDTX4lX9x4y4CFZyW7VW8H3JZ3CLX%2B2s4%2BEZE5Dm2y7Qv4E5%2BSRTJbNZgtzbAl1G1fGuLcpVlEjDGZvisZO9xXOd53q4lEgX%2Bp2XTxzvk8yy5lPHIQ%2FDMcx17ZZ5jsyVNqIaE05OKXPCRw%2Bhfn%2B52GV7mxQtQ%2FL3BAN19kqgAsPTvtsDgx3v7S0%2Bt%2FRHUIdC6MDmUT0WlA%2BYaT1WjGd5lBDcbw3Ge73FM5fdTBqa91PxxLImrspKT3jB8kpLEqPhNBus1v3jA4gD6jW2NBDMJHxmsgGOqUBwYlIbF8sWF4blHIy05OPvq8PMAHtK3W28%2FaVQRsNmdR4CKJTTRXXQJ9hcdBN482sCDL0OIxTwvUoLzD4BJnO5CsbGIZgGlJyxsU7NWrjVbO200BWfn4FLPGzjmPp%2BogGTcWEkxKZ%2FXVxpCIsV%2FUy24ZqufYwNTVVYqEfx%2FPwmusvSQ2TF7nrzCeYERJofIHFzqylhmXz8Jcth6hgb%2B1XjtVNz%2FE%2F&X-Amz-Signature=50a67e1fda1cd9a00e4500ab52357bae0d16cdceb0e67a189ae6adccf56c9d0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

