---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667J62YWC7%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCQ9F2uKfXvOW66LseauUTAAMgeMRyDjqFqLBpCbuhCuwIgQgIYSDho3J%2F5vpifR%2FaNAOselKxV13q%2BdhUveSkonl8q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDCn9foejI6qidGwglCrcA39C3blyM7dUroYIHb%2B%2F0D1beU0RJNXWMMiIcj47%2FPSWtJgHZgNv5%2FZM9GS08IfwdVWEJx7mPh5nvPmZFdzHBBxB98RE0T%2BiHG5lTy86tSm8aCNRSefiosqEFO%2BGxaZ8rSRSQq8Ktrenbxk50OB40fbM7XA1vTfJv9p%2BPskdwdE%2BTNsK29UuiU7wCf563erNL2QARQr86siwaR9QsFK%2FjHqGajjRjrCMqni7TOc8ovqhmYNXTEcWHo6gj8Nt0D5eTSKqHuKt4J471WoOEKDljr4YMrov57i5ZvllDCVQ2W8WbStXjp4d6p0WhbianOvSXOrgpcltmL9E13YSHA0689YIwTdsRHG3ZXDOB9LOO7BOP4gmbKcHHlHNq%2BkMJIDP7NxbE5lqdG5YM%2FAbACrFU6J07clVii6orVyjJAgz54FQvJiXUxRKefxZxrYCEf%2FmKMAq9vkerW0YXDJfe6yzreeqrMsa1PAxsukYTi3Os4ot4u74ZsS3jLgmmLsjYkzSNt%2BzQt6al2ndqsnXS6c%2FKb8845gLvSZY9o3dtFc9YlqZCazE8gZ%2FlkDvSbAP4p2J4M2KwM3L9qm1GV%2Fkq0bPSeWiTlMnkSMAwXuWqGqs0S6H6B0eViGeKGGHViEVMNfyocgGOqUB%2BE%2By8sNQ1UloudjlQNxMsn%2B9PvnJXRq4Dh%2BmtTk5AJSrOuWvc4GlMgKBpqGfCgaQS77UJniTD9n%2BNS24BqJ83wMUD2ut08CUkKEFPJLq4XfwERDvkD98zhn6bWYe%2BTgwOpBRpJQiV9er2xfkhnOHZRYqEOSgGNL1ucPyDuqE16%2FfaBB352qivyiXXRusYhL5a0NfpsoqME18YIs%2FcwIOWiuvSrLf&X-Amz-Signature=5410203419e5021b7e0a54867bbd04b058bc84e6f7dfd8f5af3b2eb9a440d384&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

