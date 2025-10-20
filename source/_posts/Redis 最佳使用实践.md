---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKDPVZWX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIA2dl%2FR4fd50eexECqcYW1jU9sNNlunq7dLvA3Qoczf3AiB5gA5CPzLb67Uqd8XougSbq28kdn%2B86UFlce%2BPplPNYCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqHWJ8wroxg8%2Fm3iqKtwDHoQsfs95dLQyPfaVR8%2BWTQCjM6fVdk7YP0hvEduC7iykK58xuXq81iGSQDoQnLvg8%2FartrMJgSqLWkWj9JJR423h17zquY2n7vGUz6nBFqXgIiumR1fk2S35qFsJZET%2FhxDOGr9lpcaDNuugm9u%2FchaBjLUUsgODG%2Bs0z42bRit3cFd%2BWaSLS%2B%2FRTDQGFf1f7xvXfnGM5lZAFlAbfhGCaHHCZhf6rPleLE9TqSZK7XhSUFYl3ZKtYUYviNkT9nlAHVBSLLHnCm%2FgyY34YHEjhdvQ2bSpgr869FZqhxRnjj0CRlNMaYkXjPZK2%2B6t7WZ0y54WY9WCgm9I7Fi3jLN9u6YcbJKuihIK3LijtjmETtqGTTJOg3cf9EezJerLvqwuj68CDk%2FsIvLNwtDU%2FXdrIvGbF0jmPQ8qaCSU9swCs2jMcCmVXyGqEz2o%2B6Skw9BmXXedRgoEhgnEWlxq23LumK%2BZbK6XpuuehAr2235C%2Fy8l2KUJwtBZSTdLB0BBEvXQfzEzaDFCnj7u3%2FYC8yLzN6vCL7wvbsrLGiKcJe50468mo%2Fp5uLGNlf4ogpcvvrSh%2BblFXVWdE0stupECfGAHub%2FqOXQgDJ2kYC%2BmqN2OhaRmlFAkx2av%2BE17hPkwy%2FrVxwY6pgEQUl%2F7tSwLY3EYeTkMQlHIgpIsFgVi7Qkv%2FmX1PCsuSuqUpfR2vnWV0HVdF6%2FCetgqI4pH2qGqRd%2BXR4AB1IP1ajwyRGO6GQlbPUkD1XeuIedw4lj2WPdr87gILJnsLa5NAbx%2FxSrYr9xMxVClTQqVX4%2BpGE8Fvb4090hiwHFZMMZBp3cxnfuxE%2F0BcAfZ%2BBNjYLI24OqBmcSW4I9%2FDwLhICboSLkV&X-Amz-Signature=35d5475cb9491b875bbc1a63a53d1e3b4bb5125ad334200bfe304f5f7e2f614c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

