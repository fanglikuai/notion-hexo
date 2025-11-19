---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCRFLFO%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBRIIQN6r%2FSqng%2BbQU7F1XWnpdNTgki7w5SmUZ0fkfLBAiEApSEbwK99x8TkDi%2BjGI53apkmrMed782EaQKTzMUzYNcqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPO6SBBIKl3luQs3WircA3%2BCRJzZ%2F8Ta21EFWFPieJV0jlR47fvQzh9iY5kxDNM5i548O0EerwizVuLrpohiU3miwebhh258u21nKi0rtDWYLov4EXMAMxL0KYY7edL4wclBCjQPS0lZ6Bmh8Dkwy7gcyVmTS%2B8LY88dNHPMPeubNW0%2BlZhJwAiXoV%2BX%2B2iM%2Fl8twynfP%2BtsJ4P%2BddugwngwCyqLW9G%2BMZKB30hzrALIaaBT76aav4wooDe2IiePHepK66FzQvvSPt67zF8ilUg022X16Tlb6c5Vpj2vFsZFYNy%2BJ4wvdiW7A%2B1au%2Fj5JJPqJyFWLcLLtzjhSLF7c%2BBzljNOuC1lq3y9WvMhcqL45r0OfOwKlCY62nAhHItjThpXwIjhRWl%2BjTD8AzX7Utkx2FtcuSfzagakRU5Krm1nCxrqF1907n9eaIrBxg6XQHN82pGL7lf%2Fn1Sg%2FpT2qaBXPQrelrj4JVcKS9LAbPBmAJ6YKijIOmr51HKbghZCQDf7NQzReiGBa3zEV7yB6uOo%2B5rTJE4uaGnyRX%2FIS%2FMB6GbC%2BrIuejKSmRX0ODDlrjIh2%2By1O80tNvwjulR08MMKCqBwh6dBZLbgKY4iIPDPOwVKOiThnxPD6IqJbq1bEa%2BtL%2Fw%2BYewxoXzTMOqU98gGOqUBx6v4OLFHWHiTwsH3mwDjKJkkFQchRzzGP5pDoixg12SJkF1ZdXaIhRPVY6tRBU9TLUn7d%2Boh6C2i9cylzrjzUIYLIIIddiLR2eNxKy8p%2BuqYnKMwJryI44IalXqWjsrE58ysPa5LShhUl6C3dhxM%2B%2Ba0NjTW8XPVJBeK8qHF0srN8CMPjxSqMSf%2FdYzWDIJE3lMWJh8uOBsqBxtB9AOBEa91IL5I&X-Amz-Signature=258b30865952ccf72230bd24d642d019958f309382563e3e4e78a5cdb6f6612e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

