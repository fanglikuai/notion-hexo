---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHW57GDP%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIDPqijqzeC%2FrBQj8JqqgoIVVBS7j%2FVQJyixBGWmRM90gAiBepO46pROF9RStlvn35dnempjyM4gexXFxvZRVDk3MsCr%2FAwgNEAAaDDYzNzQyMzE4MzgwNSIM1%2BiWHw5mYv4bLxmQKtwD66BFEfysSY12Xby2tuzDDva2pm%2B6BqLusabv05TMs330%2BkwV0FIW5yDsQaR%2BDNcO2lsIk24ilbgNDbON8%2FoQ%2FcDEVFLIMpgbotPAfbSQ3aQMHhvySQ9ksDvg6XgMJw7EnX0AvHxZhLlaIMehMk9lnbzxAI5%2Fc94zkd4tTIvBlgtjm%2BGjq1PLCmjnx80REreTc%2FbS%2BrQm0nHT8Tkojt%2FUfuLqdv4A4CmsyWsUAdk5qRcX8prL1mS58hyMufHh8YFSqeh5ntORSI3MnjvQh6XzaA6cGE3SHhVW98VuDy1pojvd2AdN9UbsqLP2Y7Da8jfpoPFJcCA8w1%2FXuhHe6lX4l9yMEXJFq8R9jictdmDNumsgzQtzMM1Xuxbo3uiCpiIoSa3N1nvUNfbbGCg7UJExkWXbBrB%2FDR%2BA0AEECUifzlriCZ7VjdGFwFdlCnA6raKPtssXP6tOfEVa8lr3igrhfST5pXTxLum1a5c5ztr4rouj96lMbCMtWYBOte5wBClqjpE5vBRIrGTDvic5xGoeHCKrENLQ23n3nptLvT6CHU8FqP3UYhfdkq2HF65MU9VPe0BE4cEDd731TGmiRtPbd0%2FHNhS8MxtrE3zy83zGn6bdf82Ow5024%2BeyUaswk4vJyAY6pgGAjiLDAd%2B%2F9fyz2ki0KZId5ssUwfuWcp97LQ2Bi7GtlU6l7OsW%2F9qv2BoQ2vTyLYIAX3E9glX3FHXOYmnogBDPM0YpnBOcCH%2BoecUJjSlVGbqBPjcPmE9bcXi5BYaA1zQpkC7fg%2FwNBJ9OcRUQ4TCid12BVqOsVMPQTT9Y75pyiRQ%2FvOdqqges8MocInmw2N7BYvwfCBf5pxhTp6A3FMctiLVNjb8E&X-Amz-Signature=2ca787c2cc6f4e12d384d88598a303097bcd16a1b50786cfaea62dd3c97c4d86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

