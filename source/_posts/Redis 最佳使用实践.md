---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZR3HQVTW%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJGMEQCIG7Y4K%2FrCbd7%2FWtoPmSKTTZ3PL6xobOiM3GcdZQbsom5AiAXO3ft6%2FpSxLTqdVB%2Fqum%2BBMTBhdbrBhRZqpyzmZTV%2FSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM4%2FvTnIZ0%2FGfblPGzKtwDlKW0298hiyArUsRI9ABelEFevOJBZkH%2FM4076yzKF9g1Wp62cZgiB2qMVlCR%2Fib55ElYuJKcxtN5ln7CmzXLfuWavEB1bpzfJ7MDSLHvS1iFm6ioxUK7N%2FyQ%2BkIfj0J6o5tTn6u0vFlbtoCZLp%2BZ%2FernzdRItTulB%2BdR4OiUDT6P2WmnPDQjOPI2h8WLopRU%2FDVKqiwlEfueeu8%2B%2BQM%2FrqkOcbP5M42vEsP%2FmwZjp0NtVU90fkI282kTMx1ZH2MItnNlZCIvMfFCOuYiwS94H9%2B2I2pM46MPcLlXVPFE6D98gUhQZ%2FoFLkH6bXbwq7wvoqxD4EI76MUVlKMUjk4hsjZlen43tuaQ215rqAq%2Biv5G3o7FvtfXfvTUn%2FXTItPNRTlHBgMIdkiR%2BG4WysqYdJkiuI5VqPnTIMfK8p8xRLdbRgR1m2jrXnFpfpDKmN4hKoFbsCnOyV3K3giO8OGyze74qrA6Uz9dIDQdWKtNQQEW69p2Z1OPe2ztc66jaX%2BwziPCEL2qPx3aexeBfmuqF25NwAYAtgUMy4EGgYXLiDPM1doRDAaO4VFIxoY7bDL1G47uPAVQHOrTLpBHyHLFBH%2F5%2FDjntc3SeWoEzPdQheOYuzUtZZLTV5i5s%2F8wqLv6yAY6pgHU%2BJg54hT6mU7QRzlt5vyZlDHrvx1vdFKi4sglIoVR2Uca1FnbVtjDfp2cbLC34a8mDVMfZukJRfWdb6fD2IpC6tOghLXWO8mQn32Pjz4sA1f0RB2vO6lmNBnH8%2BUBDgf%2BnsxEx1OajCnb6rE8i77JNeRf2dzQoKOsoYjVzxeWOV5kkkYnHxM8edJbHl3PVwSMR810zJTFk0EdXHP%2BPu7H32HooK32&X-Amz-Signature=c8b2425441c269ece4302daa5c37fedb96b162324cee29d5bc017884b0a94326&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

