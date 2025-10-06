---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YV7SBHP7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICmujF%2BGHHm3lOP5SW%2F2wp3Jf9L0F5c80rfAoyX%2BTd%2FuAiEA3rlTBW2U2y5Hm6D3lHEGpADi30iOXp%2BoMB%2B5MuCy5rMqiAQIlP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIydr%2BQtcqfF8LiOJCrcA7WEgkadO8sOI%2FC2OjTqo%2F2p%2BcQeKtgv2z2sNWqew0S1QD80cb2w2Qk0OCLw0w2wQpMZjwta2Ev%2Fhakj9pqOeuhMb%2FkXMBHrJuVMCndXaEaFnrwaouGd3r10YQIrhjIpKRKhdS948tg6i0xqtqM7dLYGYQXPn5obguGcjp053fP%2FEE%2FKrjS5qDwDCV6m3Rn8%2FNm8eU5D92sNHM2r1jAU85xGBUZcx%2B5tJfSaYk0jaSkHP%2Fhfg3elV%2FQV2rWrARw3PH1sJzNIViSjFcgYmQfWm1liRCI9LZ%2B%2BDnSi2GK3zRpwnvYPyk3cMkmLd5eqBT4WnHRzazhYV9rUuwgFzzsSpwUOjssZFlHlQ91zUi%2BOGHeQHwotOg5VOpCewXcMBdUpHI3aMpF%2FspGc76HGDJ7AXjRmQkQXCyNSKukf4d8r7yrJQEDutRTroJPo%2F10QW%2BjdlNgcRlFtRBr7o7JzSFqop0x029W0w1Q21aNu1ql%2FhT2QQqSVZulv872iAAW6yg4p6v%2BSAMnKnjwQoaPedUH4nM2nhLJ4qEtekFlhiWiBY1ainEhy9Vb4OMXDi4LmSH1CBiLVRE1EeErtgHm6UuTkzAdEspgrhJWASBCPVGz2XFECd%2BPj4i2Z1Ra2LiJ%2BMIiekMcGOqUBZbCoqvbSga8YuHO%2B9VhSVGwA%2BC0WZyiZwwnq3LPq%2Fh9DSNvNb4kp6JFVupCp1Pu40cxJPzJzTLqwPb2bfdshJVKkzobFZquh0cwCZ3AQ%2BJW1azucH44i2GH2afBkiTMZGSJC2W9PGooWNok1e0YlHROaSMphE%2BF6T0pdtSijjnk4fircsySvRnUXaJ4d5W3Z6iT%2B7E5Yp6Jebz0gOtOfLty%2BUix2&X-Amz-Signature=1ca658896937802de9859fd7ea21301180146b2075fab455f58ca5aaf02e2357&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

