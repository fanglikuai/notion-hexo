---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HHVP6G7%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFrW1qE0%2FeqEHiMHL7bTBaBTAj%2FVpJrI5D5OBzJ2RgdBAiEA4L6qDzi3r%2F5hpbI9SvFrKMuRbHOfcfK5ZxvPhOH0AO0q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDBRPI9Xewkfu7VtehCrcA4dBZ5P1qgl9oIpdB8n30T4kDFK%2BXAapdCIsnQL%2Ba7Esnh1dtyQ5NL2xY%2B2fJXZJcgszTIDlcpZtQRITNcsYWnPOP2%2BYGqC7I838WPuAP8ja82lKCX3QcBHu0sdPPKJvv%2Fg8cDKHdlnyIMWrD6WtoSlYzu9%2B6gDqNJ5M5rAC43HJuY7aaOdb3nL2CXLkmran%2Fx%2Fe6CINldCYOFt5ns7X0hqKGzBq1w4hhiwrPuPtjk0jmHp65UrLpFBW7y9%2BqesfAewULvpjq%2BuKv%2F4JPifjL9kcri6UNlhp%2F%2FAef%2FUBPyT3Ug8x%2BH91VvSg6FhwPiSptPe7p%2BFUiU6d62POX6LWQG19UBsVJ9WlLsdEnf8HLRNXUAkccmdA9jh9C1PWpYHKiaZ2Mmy%2BGImWqOKq1Vi%2FrBUqghMa1zqqJ3ZLhkOpHQTAKW4%2FX7yEOvVLGllXIZ64K%2BXz31Mq8%2FTXfiOE5pH%2FDlF2tk2W3oXM2z%2FGdKfcY%2FEz0bLW3igFLkWYvXrMH7oaX1xhCMZ%2FXJFr1rhQFL6%2Bqa4auAf1tmmnVlSf0vYzDCo%2FEIVZxfhIBK%2BAbk4zuPFaDYG6fLICQo9jS%2FgVbMVI2BMPmn%2FrR%2Bbym493%2FlG879njTPil1N3h6BUWiryBMJas%2FcYGOqUB8PTaT2%2BfS3PoqF9lqPp2gs8bK2q2VeAa9jOqUbuZk%2BovEVQxUK%2BKHKYTzCee3aLPlF4Ca4Z84ZKPnWwO4gL2%2FIfxHmk5l6oXJUr0Oi6pthmwAGWea1ntNxLu2HaphRwpVUBAiMGJiOiflavsjFvT3PRYsNdRCaipwEctKeEsyoYQFQWwDz3wExT7Mv0KFLtgqdNcPfaObwvc0kUpKQ%2Be%2FrpMPcLB&X-Amz-Signature=9004683b4fe777238bb1f718fd27c5a984e5f3b5510ff0f3b89c55639d00f09a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

