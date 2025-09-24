---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVZS26XH%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T080105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNmdvE8%2BUY%2FU4Aminu%2BWpdgb%2FOw%2BZ5bVKcwwLZFXSyqAiEAkp59CJGq9qpvaPnqKp4D6Goh0ltMipImsV5UmAw%2BaEAq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDCCQDJWo8jmtsmNrYircA3YEqbUBNQ9VlBM0IFEEO%2BbxK6AdZG%2BOszzNAJPnbQX12bULP2spBp291PFwdu4UsRIopkeTfAnLzQyT4KWTDNeVtx3MhuIpGL72kY2BAVtsfXSdrNjpZnVD0DitrDOCy1MfSlTbi8Nd%2FT5i%2Ba7JMVzKuJtOihrzW1xmjRjb1ANBheCMGQL1HyALqEG0ezuE8GJEHMSo6AxpiCqndq%2Ff3%2FTzrvgQ%2FnH6j3D0zLDC4TBvje6z6XC%2F5BToskA%2BhaCPj6eY8LvQMZyXONibSqXBK7z3AF80tvfmTzyMxjJngYJwyz4MUhzpCx0xYT2kn%2BWZjWBTAHoyn7YLD7Jz7V2xTREyYHntuUcb5KqocJ8RKGeIvSQc2IWPKSosKx0A18KxI11GClPOzE%2BCXAL5j3Mgyo3YB%2F7gPgYBa1%2BqcmexXJeqZGdJrvUZrycdwMh47aE6zpN4%2BX5aPqIyco2c8rHGbnyqpKcoVOZ5bhN4MPPeukou0DGhdduA5jQRak%2F29DgX4JOOcA5I3d2NeQMI03PNWSEEOJUPDGSkG6x5%2BrapldcoFg%2F%2B23orTg7SUA0h4sNVtyRxXmSBi1quCZl7lfkle%2BLgUUJW2TH3opWUAaEW9NqGId2caDuxGv%2F89%2BocMMq1zsYGOqUBfH8NGXmS1TPQ4h%2BfQZesEhOssf17WC4WD8NbxWijhZFVCwwzlpa3l853xCqC12kdjqgqhYHxQ7azhr2Ou8apbMsIYh3XxT%2FZpXQ1t8y36DsMQg4cU9AmQq0SJeUlspEV%2FyPzx92HJem%2B5azQcAP%2BklsrdxRQT7RCGIOjZWzZlIBkzyZeIPhxuj6XwDWMsfX85yp%2FKAY6JZNqot%2BMlxV1gBjQ%2Bupw&X-Amz-Signature=834cbdefffe519c2cdb8917949549e1ef35b64a9ce3e19f74f4d51f23dfc647a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

