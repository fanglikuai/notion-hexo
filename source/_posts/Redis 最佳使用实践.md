---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7RGK5U3%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIGa4xc%2FXTc%2Fics3fWgmMzUK6BrKYKldqSy1%2BoQQiy%2BdHAiEA8uFYwkT8O3Dm0m1x5loyMibQCRLeYndnj16AYLCkoTcqiAQI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCSHKHTa0CpzPV9e0ircA8UwAk47KwXx21mWWE4xTP5kVZ%2BdMGnAZnO1%2F2GF1eACa%2FJTEPxLxBI5ovF3XH1PER3ky5RJ68ZHVhtjNki%2F7wUgBa%2BAE%2BFjiIvcDYNxxsheETxTx3D8%2FM1E%2BJ9iyvARpM6yznF2ZPeA8lbmv%2FJzmqeWsVxq1lx202%2Be2FpCobjfnn%2BYR8YDb7III7XmGqU9SZrbESz3JQzzJC9mViXhKjpYywiugSOPlsUWJFS73Ge0OQhWdHd5%2BnyGmJVUBl9ed9ubryZlGmAnrHtp9sBKDDx%2BiE33gZwkkAHpHBGLZADvfAX%2FVJpE8FRj1rmpzd2gLQVQavtYTAZ0Avu4krvNmSYGndXYmkKVaoMHTeKycYXsAhKZ5MR%2BaGPS8nI6FnzKciFaiGukqJP0abYGLaflO%2BQW4olW9hqTA2ZZiobtP5B2gafWNrHtLvHDHieQFJp5V1UNgFFu0uxE%2F6XwDJEb0EpqBjAyAZVSD3mm8fBnoQZ0UXXHKd0ltptJethXk5P7%2FbPl3CsUpCibbcPQcUXuHWm87Cfu6u0e6W%2BDUjnxhbgEB%2BAK2dduHQCIseLNwTm1G%2FAMoF1ytsBYmGM6Ap5H0XyieAQE3AK3k0gs9z0noBZjsMYy%2BgFgTuakDLHfMLCxwsgGOqUBsD0X6%2FTagK6C6RKWsmmowYuSQN552MwaJ9o2U0U9S0aglWBnYbTA5juKpGMRrSdY1oBZaMBfM%2B0JAvIKg4%2FNj6%2FO44Gx310Dj5BTrYumK%2FQ40idowoIswQM%2B4ZtciJX5DxsnGsLnjfu4%2FoDu79AOICV8bHTZivf3oL0QlFuAw5Alxe5lvJa%2BY4DQL09RwgM5OjTDvjitPPh%2F0%2BE0XBQ%2Bvb7ObaRJ&X-Amz-Signature=25c4ae26086576d9944f2cdfe246db951dd5a31ab468f578dd3a9c9c8354870d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

