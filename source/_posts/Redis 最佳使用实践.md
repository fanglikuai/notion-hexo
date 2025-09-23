---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SFUPPID%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE51cv91Bs2CPbPNpka1SQex7xJQRrmDz78xJtKQpekTAiEA9EiGW0xepoOjPAj3Joth2AUjrj2fInMzVwfS3af90hIq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDMJck%2BOVjunfDFhw4ircA0wQYsJJPYBDn%2F9h0MG7cVqRnvcXx833wCN4XAun5fQJqadcDXFmnf%2FtPFpnTefEhh4%2B33KzCdSaQ5OKEhoNSzjltqDbgir8643E2E4v4NnrPghPBTRBk7YjhLvcvltNDcc9B6Ilm0kV2Ytgbt%2B3z3V1RxPzLsr2ltrkwvKlmr0EhuB7PhfKiUnBdTQY49PlAafr8M%2BD75km8bZ8wSrVStg2QGyIAH5qdOQmKwPAqtj8s3rRbuGjsciIaVwZIiGf9c6hKQpCv0dx8NbM792T8Zw0qi6x3LEglq9T15nvitgTXZozNuR%2FaPDLCX8sJtd7bbgjPuBBpOjlfLkpv7gcKcdFQ4HkSKnDvLf5UuzeB9A3Bp9DK8V1QuEt2P1JzEmKhfNBQL0e7T6Csw08%2B13NiP0qgleU2FTmryDyjUxD8VTYdtE1tij12JdJ7Hhh7nkxZs4KbF8V8Ols6HOEThoYVV4Rr3smPw8K9%2BaLrNNjBKb9u0MUp0IoBHc39YZXo%2FuCgCLUmTi4%2Fe2Tfc3rXfrdaDo2N5B2Ie9pZCPggkpPZ6POGttiVXkrwPGW5GLZUNxax8vwpv8U763Ds%2FWxG%2BZkGVd6TuGe1vEjBtNnZfOo85FAf2yqP%2Bh7gFhervWYMMaTysYGOqUBKKER%2F0QO9S8a8H2LvJsgLNM%2FofxX4R41Kno%2FlsQkWGDRcFOpmmKs%2BxyNSs7nhpDgUEX8H%2BxFcgicQu%2BjySZYmcTHMwqG4oHaqGG2hIfQPn6cpmZRz9%2BQ9kGc1ZMV9VxnGIwSvwDzpKpzWFmB%2FzneJH%2B%2BcTSqmirA%2F94eakqojdV2Cw6HMUgbLtXY%2FGRw9CEKwFKPmuOhkTMu2LNgg7omsYKXwoVa&X-Amz-Signature=3c3b3c4edaa2c01d72c609e1aa9353c2aad9d0ad0a288470f4ae1534615e7bbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

