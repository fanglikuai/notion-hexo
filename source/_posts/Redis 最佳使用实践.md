---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQVJVWKD%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T140045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8hjbvTCPTvUquXZvr3PbRShZP%2BUAmazQRH%2BJYrpyZ7wIgQp8Szyp1ciry4kdeksLk0Z9tu5DNWWmop7QU%2BI64zwwq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDM5LvACgKcDEa5DefSrcAyx7GdFjoBeQAZIfnodh4dgPtezVF6NBiMeL5adzRSqeF%2FHnRzN%2FrPTnibixpRiFJzUym0BRsWcu4udKnjaQfdsn7mjEoV%2B0HmaD8JtnlV%2BR6dDf6l4jKzWCI60OWNcn5XSYirtu9I7M3cIzvNfTPZ5PStDPXHiBbeXZFbNNHqr2IkVbpEc%2FMCo44flNgUi1XpSDcmvDl16c3LXm%2BfNFGlPKdNk2zWBQH5nsxi4XQQFaA2seXBY9aUAJ6KZ0DOztRBz4E8VullMtqZJCQVuEilKdtk9tnwopJR7oWtMqn9F25tMELFaoiKf3qkhDCMCWdvkckXzB%2BrBtRjUbE1cb%2FobwweVql8VS6KP1EHahKOmLa%2BTKnJdyIbtbDEsROA9Jtb7WGl4djHanFJK3J%2BY1gSn%2BzCKn%2FzaGmruY3ecpV6%2BKiT9KrkX3RN2WkN3pfuJ8CwAH3hQ8pBOrPY4i8HmR2%2Baocqa7SQFguW%2BpRiwtfcDDeApF55%2BCavkORw5d9Xjlm8v0YeyNqTvmx5lPK9pTK0e8fz6ucHh0OnuyXsEjyuazu0c794Eos04%2F7w5u2J8mAXEWAgzCkAjh7YkM4%2BKFNCFMFeWNBo7NsGTzfc50FUUxPMafftOsL1KhOFeNMJaD4cgGOqUBDC6Zr%2BGJikoHCy9MOREx6gp0r8NL9IaHAyRv1PBJNA4NCszZSolH2ddttqsepQQNR9kQNxic6eTWFkSRDqJpyQrOk%2F9gLbGnd7uGk4Tm64pZhSsSES0wIwU7JdlLDSEEu5632vnKBCwN4PNsWaXoQfmJdbL6K32h%2FwKw8xO7KGli8tou4LfLyfkErnhn3gCdC1A5F2HYNN6eyZ%2F18ffsrlaSlOkM&X-Amz-Signature=af15302d2c46182e01932edd686ef7cb5ace9848458c3bbe50facfb81e6dc7c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

