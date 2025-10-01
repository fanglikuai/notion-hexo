---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPZQHXSX%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDbmyMJRRhkSGr2eCy%2B1ytgBF%2Bd5dUK86lQzxEBWdzELQIgf7sj19VPtPQkfGXP3abwJyqGRH2n60IPSg%2F%2BjAPhaGQqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLK%2BBycQEqORIhsPLyrcA0b5GmOIFTX5iYCs1VCUNZFALbVoV2QeF6YXkLbpGeXHWR8LcxwaLmfRY2ohvHSBuxTlA2kqNmKjcRMyr4%2FRUeS008D7YGKJQfXJ9YabfyMgLJsuHCZGa0mq10%2BtwwyStH0hjhA%2Fohu8XK6KmHCV5aLMT3XM%2F8SddrkOaIgTFPYZWsiuSzDvpcyIRxh%2FSoIuLaIKwM5NdeDZ6f%2BN3k9UtOuhoAgTEzcrajH45h3ZpsO0vI9mtezI6lhWtwcIRTzZq%2BjQPLsXwE01E33s3ErPHV4Z9kDextxyrGKM2srM6zRpTg7xSENsrtJm84wwFRhuGYcfmEvSBqk2IlKBfPJX7b63O475FmwiYqi68ETz3R%2BIqlncGEInk%2BDqeMcqtAjtHABZa9D%2BwtfVifB9JdV2dQu6rq0OgyNoXm86VBsrRKxLwRazNJbQq3VV4PG5h%2FtAb0nJwQ8Aa5mirgLtanLi%2FHmwDydO5m5WuUXlks%2B8wMOcrbbw4Ftxb0EN5nAAMaNgET24QhtO3B%2FRfhSjnS7hy7eGPco%2BskHJQ41KRh5e2WrNwLCyursZX6a3y2WsqnO7OFbeYvcCPdBY6doP8jRlo64zOg06gn%2BPpRzaOPo6%2FVeAZMO6aUgWY%2FOsej5MMNXq8cYGOqUB0jip%2B62j4YmLRZYszSZxTBFAjyC6SUPy9iHfdvGUFHXc%2BfOJOxAm5tLRaqe3PfSx9MYaRtd%2Fo9ZRAUPo4fOi%2Fw8NVJHB3hZuit%2Fz3V4ZKRlc8PGH0%2B96ctrsFSHDAYZNg99ezHG6D0LKe31hymz%2Bo1vjcMO2nU55vc7VdAeZuNP8MTJMsaKFJBPCSYlCmisKp2P%2BflnpdfGSCtT%2FJLM1m4D6EYqD&X-Amz-Signature=10350065da59e514d6dbdff3048674a091a8fa00c795f39e2e9084dec6f61bcd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

