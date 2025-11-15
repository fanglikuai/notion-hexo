---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3J7NQX6%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUGI9eyciCX65Yo1hCDJcBq2FQ3txG53RgrVfBDA6a4gIhAJCE6dWtGX%2FuV4O1Lpu%2Bg63gMcHVp4adXCXOJCUx2gJhKogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzSo%2BM6imZgunhEvXoq3APezAYLfpRyirlQamFyLJkbWMKKih8EIiPzc42TxNB6IC2A1lMntK%2FG%2BuM0skzlzRQZP9%2BAWBZAntJ8BicR92n8vFJQOeQu0ve38ZbfH4t%2Bi7A6L2fJCRkRbcnqQv7jrrC3wbp1s6cu4wCpMvAZJGHOe%2Fm4swNNmPOOE1H57wS8uBiQJ4Av6o7O%2FFSGo0bP%2FXeGB1wkZCcN6JOe30clZI9pbRVAJIl1D5CIKAAYeEzTNPGxTMceq0CH4lQR9q1GTUvR8J8MRz9FNw4z%2B04QTBbVzhBrr8jkDDJ6uUqOPba2SVHoYv7QY%2BM3xqyIX8H59SzBvKY3iXKQGGVUK6WPd8aAzCM%2F8RVblpMDB%2B%2Bggr%2Bw2b%2BOY4hI8P9akuwRSs4W1%2B9X6sYlPUISiC2Lfsj1Izie%2BOKKtzepG9QqkSIUgnBP1%2FkTnck63SdalvPwFtTK2E%2Bkn3u%2BibsUm9K%2FCk0Lr%2BAIEKcAvl6L6uZfoY3O1qRR14qy1AJiwxk1cT5S1jPmIMqS5eHAwTTrHij%2B7od58lCFVOkAJ%2Fw62Qmi%2FBxnriDYVZwy8w8qid04va4l6LLsGaGe%2BkZdTdSsnx5yBuY%2BCqiJdQ%2BzgjBKPUd%2BJnyP2QAMpPeBN6IwJLrVOIroRTD8ouLIBjqkAYJsLkAywu0v81vf8V6RE8b1P3KnUmbMbx58ORvJqbIgjmGHE7d1TZI9s1KrdJra9JwqUS1CCHQncuVqPIqWAltRauh7bXrvzj3y84zE2WiNQ0fY9TYoxIKoO5jnd0QjonCDZwr6ft4dyh4UHxMgDz1Q3k1IeSTZSwztWVq5pGa6w0njAb9CXPXFMh%2B7LnN%2Fch9W8NWdjakgw9T6S%2Bd0Ql1to0UW&X-Amz-Signature=ea821e40c64326999ba78ae0fb94a9535863ebb51d6068c06453d504a6b269b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

