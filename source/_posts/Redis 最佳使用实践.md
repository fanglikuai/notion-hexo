---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XHICQVM2%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmCNT%2BFAzgb3myWud9zM6NS7WsAiDDMQxUcC%2BgjEHxFwIgeE0ZD1HshT%2BHjH2Lk9z%2B4%2FLThm%2BhWZ8eq9iwMMWjyhMq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDA1T99urtOuNc%2BObXCrcA1PR3LPL2fZhG6qeV02OLzYegcUKspver%2Bo%2BMrwcNCBlWq9q6rCXjOjFwssEMVn1nGUILNl6lgjlVlCfCfTwByRfDcnNPImXEyDgc5pTZ%2FMBxNM1WfuD49eNL65sOs%2BiSl75OFS4xxt8v5WW4Saog3%2F60IJh7Z%2BLRVlS8co3hECdmlUYLhqCA%2BmOQAcQ1UL%2BqPwP%2BNRyEJJWR9gKlObg0ekg27XXtflZ0uPyITdeQkZ4bvo4tEuM7pPOf%2Bdp9yJpoSdF%2B7BdkFVBj%2F6wT8jp95rtxgMrfi4uV6ncqOG9jqXZm7exQbPbO3vt862oqvbYiRfDGAGo6IpuWyby1uZTovtEeud%2BvJOHBsQ8jB%2FyRzTkhU225zPMkFdZ%2FozE1OPAXRe9Wf2R1oTHb5NwSkh2wrHmiLjZEpI2Y4xd2cZVmmnclhJpYdQnxElSAA2SUECpYfmPVLVAVNdJ%2BSg75M4RvnJlKD%2Bb9fEtRDmE9%2BMJOdFX6ZH0ryyPKj350AWwd50%2FVH0Ay%2F%2FecXAWL92b7EYX8rLDHK8pJAIn8yJ037d2YrSM088lmfU%2FX%2B63wP0LaFT37L2qZIf6iPScNr8fsAiPJgXC619Zo93a4E18IgpDtPDLs3eXDe5Jm6soHmgkMNS0pMgGOqUB3qBFv5MtYAo6b9cm42NDpdOAocczU7aUe0CX9pJkBABlx0JYli7%2BnSNTmarIf2RD40r5jXwl3fHgPPHDcvLBcKPu%2BwSPmuXIXO2%2BMh%2FJtgipTbr5bkSBva%2FS4CH9a4lCoiENinoZs4W5%2B2pLatSWzbTsGX6e22o2MoRJCS%2Fx2u9ijKRtOrwMeD9hcX34WMAGaWO9D4Zm5x3%2BapG42%2FUaX0B3H5z4&X-Amz-Signature=3da0bd03badf033b3a1c10bcf9d6367de3e511449d1f2c92f26656fdf1b2009b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

