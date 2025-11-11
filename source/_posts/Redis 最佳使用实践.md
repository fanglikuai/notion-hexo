---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPKUBOC7%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDOhbZ67Lb6pWFNE0QosUJB%2FZM2WMRKNGxRmuhPlFZm4AiEAgQ2T0u16bhIaLYxxVlC09h79CKGeAvj%2B%2FZrCzU0ByiUq%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDL5veLwVXU%2BKoWes1CrcA4H1IksJJyzzGkN%2F86KvJFx44AfCSwCINocRHeosahLbfc3MlPiCLoBNxrT2mG0w7mVnPbfwDlUje%2F%2FSfbXXsrZoX9c10so6w5VJqTQ8NiYmWozIZGbro715E4pdcmpbR2gk4MDhxZ8Oyn%2FFBppRoBFklSbqn%2BQvsW3EwK2tezEeJehDYTy574zRpYB1IgvKiBCx4eL93jSmDdN4y6Wjv0QMMVUGYU3N7OEJ553vs5BR2R7NQhzeZBdNWcTaX01MN42cGRCEaBoYvfX4SvDZMskvsX3IJla7H%2BT6U0dhyMZZ2KMZ3qvo7VbGv2T6MGW6QPAI2ekvIeHGT3BSeCWL5abUh04D57%2BfAGFrwdkrq43v5ObyjFO8OP58zB0lbN2kO%2B0pTt7h7kL6AomvoA%2BbCMP5TemsPgRFst3gjm%2Fdz7NERohaH6f1p1oKK0yEB4hFuhs1J%2FXiR1p1g0sZoe3He3GgiIDvB7Ynf%2BCCfMzBGcRELrMsVHQWrFDWlHidNCO7c8jYQjMh8ZM0pTzmkPhq9UCJDLKMR7DqtH1w1hosjXtKshsWDOqSeXmK8Op2Cb%2BSJT4E%2F8i4JkB3hs%2B9HeQ1tuRQ4aqWXyyNb2oovajR0%2FuAwcy3BxH%2FvqWkOhyGMLXBy8gGOqUBUnHhZpl9GTa3YXF3CJyrnZhJVdw6LxjpGHzAdNdo2LMNex067mfnZaoQMiQ0Wi4Be7b4jgV1%2Fw06O0tAPCHArcu%2BUevKBCik9xmKfOx9hoiwgcNJUK1T2NQhfN2h6keNFz6fq0iJoyjAIgclkHqURk2sP83r%2BDAehJbyI6QijYTcfhSvJf0gFm672hhUeIEPanpNZ2MBnYJAe9WGaor4B8pVCtXB&X-Amz-Signature=bf3f566ee76409ced54cace7579500913123d2fac5896c9ce06d67d94b1481de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

