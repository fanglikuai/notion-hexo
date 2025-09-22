---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YW7BXXP4%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0xXVN3YYG2ibkJW9bwo3pURfh7zPJmPn1SSI9gFkhiQIhAOpX2PbK%2B0h8%2BfcBEnZAaIVywzyhBMq8K49Ty7jTPu9fKv8DCC0QABoMNjM3NDIzMTgzODA1IgyQmUr7zNKC%2FLp8rqsq3AMKLpvAdlPIVZpiD40jBZg4bXMncpsbjTaaCnPPox7O%2FmzGEruYcGG8y57Xm1W5eeCFlSOhfclfWYsyM6GDqvLO1RkYihKBH7jykVzhcAT1GzOZULttNJxic%2FXu9vWFhWKOtnJTMusR3aRnVID%2FluUuma%2Fo8R4kbfdhOxkfjdpD%2BmwtVpbr3rtyq84FawBCkJkcXrscS07b5SvJ%2BYAfn%2FU3LjjRH8QTXMnz5%2B1ck9oPRpn8TEdWF7rPg96VnT2thQ1n9zxTff3lMr0DIYtsTHJ4KZX9dUydHNG%2BvxRI7z%2FFEo51IVTyUt4hS5YDvUKG6%2BFGn7TjSlB6Oj5jScIP%2FInA3wrJswTP7hVtPJKnkJWl8Ifw%2BTX1U2p0zOmbO553K6gpYmQMWVp49O%2FUoPGs3OJUAjLhGbRUBoB79Nsyt24CyTWIdXvBEdVLmtf5LBHkJBbsNAV%2BAAyxadJ4AOBDH8QImg0evlXSAaJoE5IPnunrlnwgoUhIWUCe%2BrPWB58yzNgth54wkfb11fKJMccnhoduJl2sxLVuhj9CUYKDlhfz6mGh0M7hbMjrV5I3SatKo0uSaQmeSV%2BvYQ3lwb9HsSqS2QL6FJjXV%2FL66RpErXeM45S9YQVbMX6%2FXN8KXDCo7sTGBjqkAWnR5RU5lIfxd8JKCeiBDSO58WitAdV2oZ%2BTfnSGUs5YvNkZeyo%2Bd7XIPLENoUap9W3dTbggST4xhuKRYpAJjBUN8VHB63JOsfUEkIwe4MAdD3QIgYnuiWqJLgp8FteEkFz8PLV4z43FOP7%2FcrvBK1otNdtiw3TEblup4%2FEb0YERKbkY9rpG8i8Luh2pKHQi7CA7JCtsuZmivm%2Fu38wIQ7w1jK4l&X-Amz-Signature=252b439fa4ba4e75d32b3dc04ee4ac9836389498c67a4ff90db25b285392d207&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

