---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWL3IZL%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbrtZNrmX%2BvfdTPdDRvRNP%2BvCblND1r5L7R%2FE7JoFlQAiEA25HiC2DPmx8PsIB0jbFNMHyijNglVjGbtYsvTyqMSl8qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLDIT5qgNbMnt3FbUSrcA0MtnnL%2FzHNz3bYgaZXRgFWBe5iGV%2FVzo19kMG0HXUWYmoaK8MAuIWNnZUzNHFi4CbfnuiaCmS14ytjYopuntFlY2O0%2BrauZMO4YWlLdYA9MmtQo%2BGQrepagDGQkfMQED4p%2FB54RJcEQqUPpvynD7425MznIM4Xsuacgj59y%2B8U6tkNOo3nmwI7fktoFsR3sRvoIlknTo7KFmEOPiMRExV8Mfe5GlJRoFqyUpM0zsqpGakObj2ARb%2ByeUo6nPMkTTkYHWkl5azi8O1a3WBlEuU0IeRk%2BaoD1cPbygMvRyNTxiB4SORB8n4P%2FnEerRwhwtCV%2B8bDOm4No78Zm6OXXegXDqevKcF1PI2DN3HsezdOUixg5qHY%2Fa8iuP6JhhjHENsax5AYD0vVG8f4OD2KlORtmpOA6C1xZg3q6iSj0Alytz%2F4CLObridyh%2BPY1apdIAsP0aGHwSSLpO1uZKTef%2F2C%2F6JugjhEhTIWwzlrlBDlV2q7e4hmJCpVsEaK%2FumkTo7L46JMENeC32nUa4AoQXFQHopzu3pJwqmirC7zkIJE9xeRNn1AdmwhzJHbdi8RPg7J6OlQtnfmDdBRqd94wGHSDo9bEMEFxZf3xUmR%2FNZ4B4G9Jd1YmQkZmqa1JMJWc58gGOqUB6tTTNgwGWUsnSDgoWZf3EfSAIj1T%2BEH8VNA9n2VvSJFo%2B2aUyptADRD6n6zvWwR7mQilsUb%2Fpu4d%2FyTm8xpAffkJAJ0SGZT3rJnhwozWugttxyfx%2BLhCAt3tRHOhSbp1b9X%2BYjbuInp6PnM4kn0R0ebPATqmYds1N2hEh2jwTeW%2FdqmFYGPR0cOW6%2B5roYg82WKqi0kNHZn0QjZMQsk467laI5WA&X-Amz-Signature=be21c153ebf087868dcb31400aa4c84f814a429594886444d7d21d11c74f5f5b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

