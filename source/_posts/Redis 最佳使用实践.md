---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664K2ZFEN2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIH7vDA%2BAfpg%2FOYsuI2IZmH7e6l%2BveKG0BE7Z6DbmtcTpAiEAgJ32s%2FICRf%2BgYVqFNSGdDPKZz%2Fyqm3N%2Bz1fij7Ld4ZcqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKFXneFuaYMYmwqmtyrcAyQab8Lo3ma%2FdKZj6BoMk7y5D9Iz9xk4GrlSV32SPPZqxEPdqTPZhxJm847pjSkbO%2Br5fd9puL8My67uouE9WztWlFl1YIpqk%2BEvv7vsTNLrt%2F%2BGtv8AJUWT%2BGUVyVq%2FRiPh%2Ba9Kkw8qd7vVQ%2B3mePE42so0daVJYl%2B1KXne2tsKJkLSpekItJATQOUMHT4wqgJovfAu2UC1TdsWzCOUu7MMn%2Bf8OthvVimc6KjxcK0gM6o0b53GreiKbfcHaBzGaF1eOdLVxQrLQ9hsST%2B16cBPQB54rN3Rl1Jhvast0fpa0c%2F0wxmGs2HtUWHWZaLnSd%2B0zQzKRSPGEXdHqdxduj%2BQz8FSBYLPlqdk4Eq9oiChS%2F6WG4Dt%2BCQWmPF0obCKgJE5S1ZfWh7fNar1qjdortxh%2FVbmPKmlPQWOBifRbm18QbWtps0qGXFlODL3ObWEA0a3MTE7q2kqro3OwD%2FRSbfWXoyx%2Buc%2BSI2MM%2FHqJmXHwzod%2FFYvh244oLd25%2FUEJQnTuqhE37vrYTAG1zH9DoDb1rDudfW2mPTRIekFhxLBNhZ%2FMsKiSOJGTzsqKr6PcJD2ZNhxVsebrcUjP0q1zJ8HbKyBH3RmXcVGqwaUMzkC6%2BllDO7Xmgy0hlUpMIjH8sgGOqUBE2CC1c3efXsdZvoJdvE33rrDQW3ycXX2U5pt4oWHKEFEo8EjQqm%2Fv1T1JnyGu3I02cdWqek0LzZOUBZTr1Sge%2B0jT8e0jAPsuEBSzTRP4mCnPCj9oR1xaRC7UdOusPu%2BcX0etEW64tMo5PHI1ZQ1L%2BWfxygFhDsysgTkWCm%2FyzM4IKi26Y78GlUVxmBTHKsGLoR80cxq%2Bd5omp13YBUKS8Bw0YpW&X-Amz-Signature=602ab39acd870a1222db1c387526ade01d5ac79ac8f9ca6632cec88d4c07810b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

