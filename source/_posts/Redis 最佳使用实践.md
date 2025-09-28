---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PV2Q7LM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCGfcabprnaXWTE8WVMiS8BNnQkSXJSXXkdQ4pRCaE7LwIhAMnMgY2sujYoGNQV2nK8LXJVfPpDK1AukzCHIF%2B6vIZ%2FKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwaW1k7OKDRdGM7Bv0q3ANqdBuaGGjJ7iyNJNnzxaD2y%2B3j3J6%2FMXtfTzWLpxlVItaEth%2Fi7GYVoECebd3iAwYrRwAxzf7kbeGfgFmlcGX4UIDrLgR5M17UexnG2O19jlb7oJLAvkmznHD7V311EgCQMtU9eRw3LwOOZ6qCz5lf2aWFxzXAnIe6UbDZHYITtMrfrjHOrIiBwuxPYal18f4Y5VJyrJDbAc09d4lB%2B1IWaUbMlbNY8qcN0uI5DYHslzX%2BsL17mD82R33Hl80MCGJljeUJ5Rh2%2B93fse6rGBPHiJqOA2av08dFdYyzqVbbi5wMjnSviGg2uX%2BInlf0UEqofDnxJXqpelqI2sIIpfaamD5wLnXdntxWsauGw%2BD2pmntKtQUOd%2FhWkY3PGDOiIr6S4QjnMXQXhKT6FhCBvoV2yf0io7M96Cn8w1zSQ2sqbGBN8ymtQK64TvuskpOe87kE49%2B10NefuJUYORXjQgnZITcfcg47BPkbeL26qN4JKsVB%2Bf0e%2FQrf8WApcpG3LKa9bLBMvQ2PXBRKsc4h2zXzArVuUct09It5kp4iPcrll2PTiDS9VGfiQ3MFxvXdNIzE7WVfWO4kfTlvYXdtaB%2BDLPtMLqDTgyo7AulgsTbKoh9Qm9qDE7zZ7mpNTD%2BmeLGBjqkAURZim0qBYh5l1LsK3GVG%2B76X3z%2Fgay9hsjhWOOuscLMCtLI3eeMCSDJOaREz6xFDV%2FIfNcuJHSXm36M2cojfgJ%2FFqsHDucHKyfgdqf51DJsk9xzGQnE%2FBQm1b167K49I2tCgaXtYmItue7cqDIWwN%2Bh5xBTOwf8UTyLkzBAshYt5oxtX1lBCa61m48FKG6RZlpqH1lGyqQV%2BdWe0zWhaCTDxXja&X-Amz-Signature=fd4cf5abe69623a74775fb7705f65959abe68d3572dca119f7277d252b27a416&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

