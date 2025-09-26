---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPLMATDG%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQD2gAE2K8lTYoS%2BaEDDqX02FNLhKF9KzsVzY%2FazhM8iNwIgcRjTJIu8eY%2B37SJVnfflGF7y2HrQBBG%2B4byNNK0xoBQqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9kZ3XyADciglqwRSrcA5Fv5Zvwt38ldaclNYixZWo4HoJP%2Bia4ZZDSX%2Bg%2BtBglAq1YvxUuZNhxIGRbvwAdMEizQzu2WbBO3hqCL0oUeQjFn1GGRKu1D3FQugpXlp5Cl04y6g2R6tVZ%2FGhlVsa5RdO28aeActN4M34acTA%2FQyRPkmx3iwKJuAh0Gih2b%2BnP5vmtvXc246bRxz0bKF09AHybGW3v3BO0G1LmrM4mrMY2yda%2FzsqFam%2FSchYH7fvQVyBYIAlB8KQKURT49UxrCMwKvsI7I7o9asFQ%2FWZ49XXEoGJm6SiPYJVDwfj50OUxk5DWEiGY%2Ba3xe1axNy6u6769tN9FhFS8RiqTWZ209StXMeMw639u47MIQaX%2FhlWYT4mhOmiaSelcLqQKhsf8c0QGDF2xSa8rwm%2FwZDN75Kil1LOi46fR%2FNpxNwHCjDCzPnazZdjsNuZ4Fxn5e0hNlM1Mn8EFDpPPWWCoePWhkmVdK4dFOVbKoodL3fdLEpsAxvQYU5A9Fw%2Fj8gAVmDNz0XB0PGB2jpoV7%2BG2ewXfAUXyzWkYKiPxr6Tz0NggFdiU6Y472PdseQofiLxBPapX72CJx9KvyQC1I6EBac8TzukImO8lPC9mInNgbI7x8%2B1oqaN9nHvUKhEW1N9hMN3n2sYGOqUBRe%2B93iysnO5aa4wgnvE9flcc4NxF3kwLi6CUgF1k030ga2ty41UEu4mPkN7NFx6UqMfv%2BLjfraLncVo%2F7%2Fw7yy4X10mn8CnymsVna7mQ0kCmWaLBmRCEL7aqYxYpdSzgTxFb1QxSq1z%2BmbJFJge7oi0dEiLoiJ%2B3mQu%2BK%2BZtQxVx%2FtW6X83gHA3cpQPVei%2B3Ww6wfRmGVZEsCWob%2B0SyRrvrBPlg&X-Amz-Signature=25646e9c9ab48240a42215ece4a8701093bc6f878fa730e504cf75a2d5fed5fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

