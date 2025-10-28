---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YABPNZSF%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDJkYWLuvb53uIESQhbhE55w6sVYxoIH1QjWHaNqTcYzgIgSWhi58a9bSUMnbVWJdE1vfHMRq33k%2B3NgUW8XPdLKKgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMASSMhTkldupvZsQCrcA2UjQvKwI3bbCiv%2B49djzJmdipMcbMImvkFF%2FZS6MQs0e9PHhHE3r3Z9tnFYU0qfA9QwiZQQ4MelZJKl%2B4%2Bw1z0LMVoICt%2FChaqr%2FyR8i9hQtrmX6b89McF816i6eyU%2F2d6WGaTar1dUGSMDWSf8Cxjn9D%2BToAr4ZqJgLHsi107zlq9aQOuEblya91b1nP9wn5wMQababF9qDTPMsQNHNSyvOT0fZL%2BsoO28qCQ2s7FTO1rrbqrCjEMVNR7gJ8LR0Y9OonKZHRJ5vp0hMdLLG2TDsTmImHrQYkvEwDn8%2FjSmL6Se5nALD3O0IPC%2FIVfKBFQAMzQxq5C76%2B5NdAo3jE1DkYpp75CvddpP1HP0PjThODHuwQeL0R4HSwMXURhG1QHMaOUyIaMu3YywO9cslXBGZi7eywpxxNZZOPCTfKsfU0k1ECI6XA38saQgFA15YRlsDwP95KUFCm%2FcgO%2FP7U34SZK8LjihQsMooYlSuyfvvHIN66OqrddfMTdDtVBzN62b6T0gNIcIUxc3a5j3lGrVNSsJ8ga1kqdRojAvm7rNEQCnT08E%2F2mRTYlrsaHwHcSrrrqBpRoEfyCntVN75SKad9TMUlldeISi4kWAB%2BzrXsHZlCGtYlL4jP8GMKOThcgGOqUBmU%2BO5xt1ArCkh08hAaTAaK%2Bs94tf%2FlkXzG%2BSHcFSR4KMWm0%2B2beXYb0NOJerSgyc5xr0AoxPRCqgkfZd5zbeKMnpQqA02pwBNiLVNFgG5me0pSxgqWK7kQVLIPMjcFK%2F9C4M%2B9JcIlmgRKdI1eNvL0xEVeJLRnIQBbBYe%2BqlENdO8M7RKiv1G4H4oS1VK503s4quF%2FoWNb2dcIGe4zGfPdi1h5hF&X-Amz-Signature=0c533f48483ae9c3eb832cdede8de965f3f1178138af50a8a66dabcd4b49774d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

