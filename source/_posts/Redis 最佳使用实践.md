---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HKUNUME%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCbu3VtJJM6nqTK8xUSnz0N8LMAFYY3oXOHn2BNElIg%2FwIgNbiFuQ1z4%2FGMeSeA9WLGEASMA8hCMXPTSoWl%2F%2FXijQ0qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLVXt9uSM5Uon2ZlrCrcAyp16EMvOc9D6zQhIM%2Fot4YTDk2MLxhGqUqb5BXnpxno3xAp5RbUaLMN4MyQDC9DxNKyFu4MjGAks0BWwa68uC%2B2IrLGVSHcEKkq9lbL%2FnKoXCKHTdEco99k1NgKE2hXGuTdRfmgC1OUowX1Irrym6%2BLi4nkFu%2FQSPLtcu4vpeM17%2BGEDGOPg%2FYoGff4v2oaCpsABEecoxr%2B%2FIqy91%2FfKT0YSMZG7De6USg%2B3dkiJePfVf9XRSVtdIWls0ueLxkGU3Ud%2FS7rKbS7Fiv6YrfYmLskAhM8PpkcKUE5kQiMXvBwjavli8YQefz1Zg2JwF1Olx2LLlkXSaEcCicEdqHzm8x42AG98OuwYi3xLp9d%2Fh01y4kg6dnO4jK0aSOzE5yn4ANMF8aF5XjlighaEYn3B4lYG%2B%2FhK0dGj8CFi%2B1jdT6evvHcKReBX9M8EUBR5xYTb9nUzwBnT0%2B9JS8u1d6SaIjncQesLEPFbdOnklvKVifHRc5ra7YOOSTl6qXQ3rWrCdsU2hGBje4CceAFESU0RYcTxnhiVP3%2F8SOVUAtb%2F3RL7HOVR3jUOvndgvaw2scj4ucJwGEOtUMFcUdCQMr%2BhuhplGqEFbyXZBVaQZXXre5GZ6%2Bo4q7l08RFNi%2FUMLe2rsYGOqUBTfAMQV1mtCYB6bG%2F5x3HBdcr2d5i2E1jxT30eIwg%2FHsimMUf7%2F9hUhGDemuOnQgEgPHmD3oWj5L%2Fzqkd8KkdqAz1KHMd55AXXY3nWddeJb9iSp7WqNyOIOdWQoOt4ccvm2n4tFBDesTBkHsjaxCzoEQb%2BrFxb0loNSxcNR4nq%2BvF%2FuTQAWMnPJsEz%2BpNTLXLvFBuA8pRbKL3EAdl0f4qGDtwAvD9&X-Amz-Signature=e57e5d436771441b0bca8bdb43777df1fa1bc07ce9cf42f13374147acbef530d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

