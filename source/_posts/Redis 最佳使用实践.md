---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z5S4JIEG%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIDWJ5JOZQjSvJhp%2BIIE%2FRygywqImWvUlrXmh1YEIbDQTAiBJULsr844QsalM2jhm2lF4%2F9xKfBINcVq%2F%2FEcu3uGSbiqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxytuFaddy%2F4K9SwzKtwDKT%2BSB42PDZgTbbysfq1BS4zFCh1ECoOgcyo%2BAAMPuM4qdx1e%2Fr5jY6BH%2BVj5BbtsETnbe6%2BrEqemnkOLFdxwGqBh7c8HXlYcQQJZnuvlkSC%2BtGS1VhZuCSVBx4k5cw0z5FDgGHihIOXNHxK1E2x0cuu82XviCysqwKg38gY%2F2a03lvWfEu3mE2oRd33rPvv431oC1XTpzxUP837%2ByVI9OqYzqc6UZeZaG5dmeTC3UNcRwjxkJD38R5Cq6yy9H%2FM6q04vOGub64twewBcQby%2Fd6T4LS26uEmOJKwVUp07xsSPMY5tlQM4%2F%2BaWqgYZti9qsrdZwCctB9%2F1%2FuiG2irRmysbE%2BRoAnREFZf6Dvvr3ogBdAqrbed1D%2FdGclF4PwNNNnjxsGRvVg6N61ldfOZtFVL%2FUb9WHpMhjitQVBsnNXXYSTDlDk6SuPZ3HA%2BDu1zQAjCIjHw2%2FeWRZegH7X%2BYOrsi3ZBaQ%2BpjpKI3QkqSW%2Bsv0DG58yGz4wo5HCA3jf9vlFWc5FVUbtasXVrrGzCk107KyqTyizY9pYThyFPYAt99SeUZQD8dYQt%2B7cudjXsGQdHInzvWuXs9rh2n6c2fw2ULL6H2VNzsdp%2FVefXXy%2Bv8hfVMcVRehUPfVNEwxabTxwY6pgFJZ4qSyFfSqRA8pN98OVXEMDY57xD0%2FL4klh35NCsuS6VtB9RlmRRAsVAvl5A6EDglkiNF0stjvGOcDJGjn%2FrxAiYT7OM4wUli5pIx8uC5Ing5hVjpwA8XeIh43OFMW2sjXz1EPJITgIjKOouUUGL27L22jfyDDfX4bRx%2FWgqJQajJI55wjXfGodIZCljD3q0SF3pblyFsGB1bI2Yg8wbDH9YMhyYH&X-Amz-Signature=34ad06e627be9030c96be0960d047a83cba339b172e693065d8b2eb41f034e93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

