---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QJ43LJH%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEsaw9FOXd7mvI%2B2H0wL7bW0HIeydzzR6ztSlNxmUzDxAiB7E5y%2Bnbr7f733Dzw4Mkd7kW9Ser6RJZbhtJSWfHCjoyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIML3ZQL2AyMnW2NJ4ZKtwDkjLf69FxZ2yFsKqCbBll7Pa8AoF6AQUJhqL9RBjbAV621AwcbZAAZa7Hby%2Fk%2BrL%2FwRtX0pde2gZWq6GtpIFqQ%2FIo8Edm8mgef3P4vibHDZqNXa6FaPunNAfANgTgZzY2UrjuMq3jrMMCrc3dUjvVDenvenc8fTHmk1bg8G0FQl8yY5MSMcn2t382EjTGTIKTR5ObD8bHoKPsvmLx8%2B52gXIq2%2B1JeApoQLXXRZFtWKtHF3RYj6x1zqZJNpwQlOfMknloafp6UitfqnHcdsigvLTesRVp48IkPyFhetmWR6dOsGpRMXfm16mlDF%2FE0DlBb8EU3D%2FjDayBQqHLicy3%2F2h0KaZWkvuCjOitxaq40fgQBXO92IR1WLoS9aGDLIDttFc%2FLnZ5rrlS5TWStnCJB2LDgg5Y%2F%2FX7LfcFLQr8GdvF1Xp82DtRLj5S4BF9oCp1C423VD%2B6NVbFS6Ba7OJal5xq2fxtFLR29NpEa%2B643w2aB1IdzC7VPdig%2BVagqLi28%2BnTuq9RK8ix%2BdBeHfIDF8%2FAHvAxtdagrGTUvSSVFWJknaTpnSSLwjnK9PY4ACXfhpR2U8BeHb2iKZcKuWsyDRg%2BIWCY2hRwA5hUqK17J%2BOVQHLYyD2asS95Hhswkb%2F9xwY6pgFCY%2BjbOQfHg4SC95AvgIgyvq1Jx1UtpixQH%2Bccya6OtGHkf9C2FJbefKJ6eGJjKRje24FTXhzYk%2FxuoZjOcoqIM%2B9I3PShULOaKc%2FNH%2FFJDHf8B9e2yo0GPu4zinEYH5MdP79d9Aj7f0%2BgkaKnAPDt%2Fr4bqF4Jj0CcQ0Vk%2BTfraIi5bI78ZFPhH8%2BtZDKq%2FeCwnqbNoXhfFSEaUwAjii8nsMAwdAXK&X-Amz-Signature=f42bde57e5746aac1ba7330ec88b55c20260a43d54571ab6afeb88d426fd96f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

