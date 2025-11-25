---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMB2KVPI%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbKmNBSzT5v%2BRdaOPVe3M9IAhW6GHEID0ve8ZCtr%2BjQAiEAnVWR1GgaiLLOIr17YAL2CdhkDHf0jlRurZkSmd6Pg%2Foq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDB%2FWr5hoMeKkfiYY9SrcAxUv8FOC9A1arl699K%2BFRlmoqjOYvbjenW1bCRGM7pudf3cfNouvxgiH2u7ExyHZFCxm%2BPyeXjMxiKq56hddog2lL8URlGorWrH4hGOtU7OAYXYIXSAb6np6f7phI%2FrW0KZ4FhDUn07xcxtkPL7YwEJHVArPT5ilcmwBDYj6Xw34BiNf%2BwsLeQDxMP9HMAkLQpfM8lLPyluJNJuNjByweCcNEWVF4fIjnXK6EzMhZwJTjUeIk6z1BMz8sjpKzCQYaCKwwnixRlhCY7No6Lt98%2Frk86IXOxGNbNofFVuNybJnejUChTZgsHUPQtfEr76PvHSWs%2FpKFSLPgrop68%2FdUpzFd4jydC%2F1oEoX4RLQVloHmpXgy3yJ4SjJNxTfqr0PDHCgJyWhF1tRWMsvwpxg9awtYCDd6htorA2K1dQYNPl42ohEOMPLYDqYcgcS2tLcbllxiTwXgeLjSW0j10D8ysmbnf3JgDCXhzTLTmPfEWsUMdDheXAAg%2B24JgW3sojXJ8NNY4HPZsq8OPVNuXYr3WdaW9PIhV5vebr6Jtzl%2BSXBApc2Tb46DHyn40Bi99rEBWBm%2FrZaMaU0E%2Bk70nEqpXjiX3Lt%2FV7tdBi7dtAGVJAsy68ecu8nKwWshp8xMNmmmMkGOqUBhymAmlaKbGbUJx1E1zhGMakDGorvAOjImZb3f7Hu9bGbfBkdYCSo%2FF0PnuoH%2B%2BOkBmAnw8OprZomUYzZxSY8fTT2nw3HwLIgEadPVCJ3P1FhKUkaPj5%2BG8%2FqlIVPI4jD%2FvwEPD33OCSkPf7c5jaWQNkZJgg7twQU1yIg9n7anWTZyG%2FF%2FBXxwxc5xsjN0UstaDlA07HEtUcOiqJTPFZiaIjPrP71&X-Amz-Signature=d4c3029ede27f560570883a5e721a0885be3fd9f1f6e3bd2c933e7dae3dde2ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

