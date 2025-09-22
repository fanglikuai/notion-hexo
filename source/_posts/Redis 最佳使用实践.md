---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V26M7WT2%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHJ483PcWB%2Fid36DtKMxMe9KDyYxyMDl4xW%2FSb2kEx6AAiEA5daK78KpSHg7I0UfCIN8TJaeGCPDRYZJtQj1%2BGGeHvoq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDMr3V%2F5Rnu7luFOvPircA71M%2FYmDS1cpybDR8LWhrITeorjSE%2FR8w2wgbKS974mp5Exl8Q4fhP6dpr9QIsxPUqF5CWrqQ2v2kuT%2B2ZGFi1fSKT2MshzvJFFTabtp1vl6Y3FOZ9xii3sbWaU5IO%2FPL2I72t7WshW35vIIML5TCrOP1BgEkDejNaM9w3T6V7nmSaK9OPfitSzHOnUL%2Bf8tineuTuCfEruyiNHGUlKihIiQhKvsZX8EJdmWENyY1Bks2gav0XuHSlHJPM8MZJZZ9iAd8U0BBKuLm4525NX4OOzN4IAj4khJyEZfHQPMfV8u5XRhIiy2xx42bOOV5NWESBsq2twTTCBTvo4s8uNwlFW2SSUKCpk%2BVal5l2p9wJ1TbGKTvEneddSAIQuOm%2Fnqn3R7PSpU3mwFL41b%2B149dfcwWsNlVs0ygp4wrLXVD8D2rMLaVYRN6wtbdIZ7s7hPAhtHjcYfe8gKlQZaurIbygw%2FxaVJLqLpNO87vp1DyOF5TDO%2FQUKiItcFdQiE6OHYBblto6mNP2IcSllPPsFPl1zrb1X%2FGo0TURbRHE5ZnVQCIkgjgTbslky2h3cm2xni6IdywMpyyprkZbRMYsteYtlzKipcG4CXvLWvMW3HemABiupNU00Sz7DtdPv5MMygxsYGOqUB1DEwi%2FX4hZd4ObazRvKezYfmTN0ixM22ZrLYvr%2F5OhjwwZkfdyNJjID0N%2BOzbFFOyODlmuFFCxMKFyZxhDv6eFZ7a4Nw3K4RgGGeIIfY%2Bjhl7HIFIt5ny3Op6%2B%2BuT%2Fr84DAdBDHXNntIm9rSREwXwXlv1Kt7yCwZXLgtGCvBAa%2FZzNGXmVJZ5gb041mBcUNeDdIr1iaS3a8BsLf5WjKYA65WlnMV&X-Amz-Signature=bc76d4f36e515a5b79a04d527358df02719b1762be9f2acb880cd4f948a2c1f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

