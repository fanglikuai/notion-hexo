---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRYL7M2N%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T060056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDk2%2FgcSFCCuw2ZpuIktyKh7gL7fGt4y9nueVaHnC71wgIgMRzIJUtR49VRRjbHaMTQal3CF2cg%2B6Wl8z0jg2uXuYAq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDA1Vhp3P2NY3Qj7RDCrcA6GM4PwlHAGNxS4%2BELFBNbA2GntRZJh8ZxxHiwR4kyuvQnN4OzE%2BHZcIcsYTtGSwCIvk1z6n%2Fz6upuvPzEKsHQncI%2BwinFWNv0tdAH2sOUx6IOQkJzJpxhRhFc2PemyEy4iW7iDCpnBmF%2FEi09AIpyiA6yrcNJLgKvP2Ci8%2FjTx8Tult%2BAly9vYQW0JRh6ntLmmbd7KYXX3LCHMsGl7VLBd3XUhzJxj7st6O51f2f1RYSRmNfTw2XVoMkf2GAkr09jkLRSYQxGBJ4uWNamkNsC41NdPcB2ZgE8hQCdyoiGKr25kN8d2da4edOhlfFyc567D0rtmbo5XJD9eECL1GEeAnSh1Lgzchurj9dZBzknNcjPO9MK0kRhdd0AgwHCaZc0DM0BP%2BKoVczion4DrzkKgUdDuEcXeiAZKHJA%2BEizGe7t7cVdAjBF%2Ba3Urt5fVBVtskbJHeXT2xzY3%2BpQCTBa29I2kNfeGqEyi6vUavzHr6CcTTvpOZNg5WoQ2ObS4%2FwME653RBws04sT1QXTLAStXCb2%2BaedZ0fk%2BSVzOrVLzWytChC4vLA7mcCeWZUZxMxKAUni0SuL%2BRL9udeViEs8W6fCy02uLSaf5lfkehOrTZZkeHDL%2Fnw5JYnn3wMNPJ8ccGOqUB1z22EQZ9nMKHyuOfY7sZ8C4txL1%2F3sQWZROHBTx5Eego828W0bmR%2FLEp6z7tfVPDCBDEMVPXfiyq7ta6fyhziA%2FN7XLz7itEH6%2FxbZplMlfNctSHKSJyYAfmIdYYkJwnuaMISK8h0WCgoJpkLdqp0Qt9xtu%2FY%2FTNzLa6ookefz6%2BGUB4ASMOr7Hwbg4%2B2Gc7lHlb%2B9a8thZHcnXV8XiyEqXMnXF7&X-Amz-Signature=e30e467ae41395750d849486440b771e194d27665de1e8f6de19be3eae239791&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

