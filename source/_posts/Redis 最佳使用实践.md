---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STEDQ2QK%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T200052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQDG%2BteVO%2BpmRqzG0p61YGgEcLKXSF%2FQPtq8GXAU6bykdwIgWMzHWc%2BRWBlnJsh8omOQBbOLRj0SuoCrVEHxr4C%2Bmqsq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDK1IABTxFE5FONMApyrcA4FcRRZ0kdeFD6kbYXH2EnNZo2J7MpYm7ZC%2FBv0KxqPP3L3fr04t8idtJUk7Cn98vaswXzZp1Dhsy1nAOM2iCG1VlejVMwE2d3otRg5%2Bf68GeeUvjoXBnHe6mw6i3w%2BfRc8gBCaxyrjI7TwKJHkLQ9TOTMXxYLSMnsfmAk3mbFeh19UCYNCY6uGHXAk%2FIB9IjJO936PkqAm2HV38Imw6hdTXFOYA2bfUBZ1%2FGLRC77z4xaVcxVls%2BIsZyCARfs4E0GeHuu1XycyggB8ot%2FmNHm9W9DwdziI4q%2BdEu5fvY2Dg53PUsjbfvy%2BvYXSGYeWWpW2R4fsLEk9oloS52r5Akt7Cy%2Bh4BPSeHCB98hvfEJCoqucBSiQAJV4uihoCVvz5tjNhKYanTGIRigq3cUsb6owbjzmVaeh%2FwAdJWaJyJtiMayEE0aty2mDfIcEE3HHY6qWiAQ4okppuExt%2F8VMOU8NRkBshy3efhhM%2FuFbSyWV%2BrAe523TZuhRu4kcZJNXfR%2BPj4wI5RkEtiF62RLq2oCaEX%2Br1RpiJlcNzh6yvLZmuBRmlgG%2Fn%2FqFq7a%2BlOtrYIz5uI%2BW0zgCAhfvZsl7mCqbYQgEJr1Na5uTJ9GbWMFyZJco2jgzYuCn%2F5jW0MI%2Bz38cGOqUB3TKfP4Uu2pFuJiI32i6UEtQEyouTleIO1H5SSYJG9HqlnxjaGGSmaXFKqx9RF%2FcouQLSD9%2BcKDYG2SsestpusfGKPouE%2FBeFtHYyq3Uw3Ht8T5ZQ22sY6v5alPOJ9T1LjEdoxnCIf%2FSLP7h7ASR35W2P3p5nRljqPpkFfhnkdbfdjtc1Ua1VzGwcooIC1CahWvHFjLicLE6dTrJkGmbUvdI49sY2&X-Amz-Signature=b1ba21cbdc5d6e96c655b1279c989ab33c273e6ff1c982e651ccb1071f269565&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

