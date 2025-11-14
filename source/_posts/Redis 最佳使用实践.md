---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHLHPLZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIANR9QDL4dBUGvlk4utpFxAI3hzAIVALJRZbc186S6N7AiEA3%2BwSBVniUqjKPd9N2bCtQL3WYpR25wX9rId47dunMLUq%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDChhenBz%2BIaHXeUzBSrcA4tASmYOCPjwm%2BiFvuMPqUC6rCF7HaqFp6f1lLbq4j1yCk7GkkX0VOBN6lL2GOG3NRkoB1%2FF6qj6ZkES0F464PeuvfAHLCaHGFIARCXk6QWVdVTUqSgwpPiNEenB%2B5TOqLgnfKv%2FJylhfoFaWFQQr9EeRmdlNWf8cH1XatMZDhCZrE8Ilm9%2FweMij%2FIzQ9%2Fo%2BjJsIA9K%2B5eUndKs4FN7C7E7Iy1xGP%2B5FOuB9BE3%2BiYeFPNQPW%2FYJTpU%2FLkeZF8eHXelwOQHD%2BTzVzCz9PvdhcXBCn0Ek6xkkBsLfM4Zvidf1emlNbqEjHoSwcNIt%2B33PiaqCa%2Fq%2Ba73N9SSdi%2Fw51idMYwDQGZpmgmV1XWoycbMGAlFhWEBXXogJQ%2BVO%2F3o52WxSMG1%2Fx1T%2FdbGKhLVj9hyIjA2hirFQDQf9NB2NJZnpfcraUOnTxrvj85bFchNV6%2FvsUesTK33PKqG2eAtZHtdh8p%2BLBPOr6rugHntTwYr94Ed890538QsTvmJ7jpIx0Bw9ADbGhxKlWrF2CtscZGYxSyXBr2r0GL5nhdgh1iFt%2FMkCoxiM%2F24HZTHVmMHoM0gCqPrVoEw9S9bfDp82MeG6ZqIslaaDK6s7WUz96kr5fRK2o4Q%2Bw3s80P5MPC93MgGOqUBM6HWhsCERQaYzgKSAeT7wznKsFlXQi4yhwpVYpgba7LeMGXN%2FBx7ZsAOO6jDagc0Zw%2FUI0PwImOC%2BIoi6c5JGHT9hDinIFuZCi%2BhroKofXJzbi1iyoAKPjYRqvMEYou60bbPdthF31iSPj6epnRsEkkHsB1lXfFEFG5bP5Ufrl2oYIy1VrCzT2UcbcPirH4fUfQY52xAYkTFBspcBGnzgnXyxZDf&X-Amz-Signature=dd4cc369b4827b9cc3e18b2c81f192ed4c95063d8353893e970992ce26c3605a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

