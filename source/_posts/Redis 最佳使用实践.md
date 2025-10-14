---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZRY7JRV%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICbKvsBr6to503fZI%2BZqX389N2TiWgzBa43I8V4UZJ25AiAUzC%2BEeh%2FJkf0l3WT85ITqgbs1peD7vb3OlB0WY%2BPTvir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMfAtzrDO8QNXJZ9SyKtwDUKD2vE%2FzFAxbO3R5lAS1FLzkop%2BqdGKytEHsFzdqKTx9qb90S1ZJLRNBIzja7Tu0zxUYQ4NbhZ7m1xKaiY06BomLaBgfKMQxccggEmOKtM59Eqg%2FH89M5XDzoL1oFg6peazVMVgesubUVoJWpawMmodzNanOexO%2BM1iopfdbw%2FcpMiCQ3ZUbNuQtynqCFEHO2Ctn1dYnDCDHIdwOXtUvm0hCE%2BRSGt1LE5s0Bht2L4hS4E5Qnrg5HUySOJ5MR%2BZxg6NNN7CWBDUo1yF7sdYjtxPgTQ%2B7GxrsHSp%2B3GCEaSp6i7ETwxhCkSHF0t6xg%2Fe0uFabNDlxA2ItN9dYF6MSl61yJaudlalDjL3AEBtPe0%2BMvF6q1Go9QB%2FebyAcqjjxYWvNxLsFSI7JI0iUeCo%2BIjAvcFUEennZDa4YkrqVpaalJCy4I%2BKqKwIDf0MQBFfeN%2BUhSja7aaXBuzjv7n3CsI0w%2FR%2FOtNnXszIYuxWToJ6ezUm8TaL2bG8UNdvcMX%2ByNxlI1o%2B0t767lMxN29ZWH0yzln8iqaVt7X02sFi80v8872su9MIASKXqyIFSuIC1FK26s%2FIJKxi9foMBeHYEQnLdyYG1VkpNXFaxz%2BWf8Sj3wudxSijYmMl1mRgwptm5xwY6pgEa5Ly9LUrSIKlx3TD5wsZyBJGGgLtisdVukUr9OJRchS8Sg0jS4zdJXofmLsMBI2nHHbxUyo%2FNtFNYaqhW72O6sUIAd%2FdVA%2Fz9mzbb%2Fl4nQoCnXNHeTXz7N8rKM5rOjcLjR%2Bi2VjuBxBgijzW96OZrZynREZZrW%2BZuGhZZOmSTzyqbO6WLG7MO2GT5PQN0uaxfRh%2FKGkMuRJ6NsHNO%2Fp7%2F21IFftId&X-Amz-Signature=60b6a978bc24f7732051b023010b1b58455108bc639f213770b05bc737572bee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

