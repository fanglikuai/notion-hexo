---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RI6LYSRH%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCIQC6K1Ro1vDSuygUoO7X5MyFnkBlGjC%2BiejLqDZkvgziGwIgZ1nt3CBc7hxq7XObQuRpUstLs%2FdR6joNdwlPDYpcmj4qiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNkfgqIfbw4Y5SnghSrcAytzTlqVeC2aftb8Dlq4bchtrhgQMEPr7km20LMWinkhgu1SRE7XgUqNCEHSjZw9D5%2BRNSzieLYaSnqnrvMebTvvk3438UaT4Xlfo6QLGJcurXlMjXTAugCpimpfGpHWPA3%2BK716fDn0VkpwkG4sHQLiBUkvlWyJCKEnl%2BfaIO480z7JOLCHa%2B2oAxSin8Y4scnT6Ir%2BaNEoBbBRcbkoHjBrEOLo0SBs8L4FxU898vhoeFfUAwDJL012XzSOZh7YS6EvNgxiOsSuIzeEbtauQMlPQIlxuTxdDyB7vsWnJadKwyJuIwfzg3FXLbuvyUGA1wA5oSbLHaAxZakgxTPZ5OUx%2B93NgrpxOmuYs9Asn%2BbeseXfullSdjxhw%2FHx0mcVsexP1ph9NXqBZA8kncQC2Leybj9%2F1S57bboUurSVD9%2FJDz8NhiKHk5MlvYygby0gimur89z3OQZteW%2B9ishAqc2ju9o%2FPDOeW9QNSwMucf0kfD1TfSdUFcIcdC7sIcnfa9hfOmXOAGvFek%2FvmMw1RLvyUDlYhWDg9Ql67iP0f21TrdzVc0QWgDVGvXKIT4hN4hHGwOhGd2vWCD9H8PWgny5X6U%2Bf6h9xwQDfhocdEOZDpqk76D2ZHYNaROWuMK%2BkucYGOqUB%2FlafnS1aCRWsWrx18KShbgICG0ikB%2B7SkJilN5r6Otlz%2FJ%2BtsOFIk%2Fn76H0mtCpxGSar%2Fv10NdE8ptyF%2BHsoIXCEsr24nYxcCH2OPNUQq%2Bf351tJMSZB54jtcknOhMQHwXJE99EL38v2AGq3PG9YNyUvj0gt8HDansK2%2BzuL0iJSmbAOLLwJL2irYdsxw4QTiyciwgE7jD%2FS4d5hUOoogYG8S8cG&X-Amz-Signature=4f07a55e3d5083cd8a59a08f3cff16dc8736ee05917b826e7602d7283bd9ed8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

