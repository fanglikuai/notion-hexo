---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5BUPNMY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T155517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIBSNMuLAV1WXWaSXE9vSdWHJQEvIVJsOdHpW6xgN5ISJAiAfMCOulbIRa26FB%2FTjGGrGfgGEFZpM%2Fzz7uEUIhmYnASqIBAiQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUP3kmBQGida1GHtjKtwDTtu3MAZzV1kZkqKickDfSl29RCFopbkh73FWGZPQExln7Go933fq8Bi9gGtcaX0zaN505nZ8MQFe7wsnbdCUGLeFgWGJymUPhBr7qByHX1238PCz5tmM3PHbXmGOasUikxVo%2BlHdoYgJlFJptyr8gGGItjg97Fz6mpoeBQl8lQmJ29Ta0Ob%2FUAwL%2B%2Fvbiw6j6JbTcD0xhYoDZjiX3SFeaJR1op8WoiOUKmL3IMKIA%2BC8xx4Fg%2BJIm83Oj%2BOENolnNRlfi1%2Bt7BUiiiwhSu4U%2FUqstPHBxzoV8jzaSGMRU22cuZPEdPUMnKwCzGUiE7GuFwoXqlDw9pf8Q1cw90LFl6RI12aecUDU3LQgPVRA60TMihYu2YLJZnAK1iNd4dunRmuRIP37hTbgaWGI2Dy0KJYMRIVGb%2BUfIVA787N6kbN1tGVoqlaWcamoo4aQcyFG6IaCJYAZuXmzXWH8OT1fkcgWbC99iQDNztrL6uE97QNks8qp14DOlE%2FWsy3z0o3%2BOiaypNYvArp6mVwxbala6IuwJ8G4g8eaHpQtHmSpl8ZqVz4r%2FW40d6wGFg04HcLyqvgoSaxcyL2e97Ws4Vuuh%2Bq3kqx4KmAfPoxDFPdf1DdJ72DXYCGbrqoP%2FFEwsuilxgY6pgEn2d7%2Fusx17eJRT7soIC8REVKjRVdnIB2xfieSFxxQR4TdtBJvtMaQWFFqrclsE0Hy8LRxaV40GFd8Fs3Iztw2Et9X6%2Fe%2FIZT3a3b2O%2BFlF1GsJ8DnugTkS1l8SBInd63qD5whBt9cla%2BhCuspv1UZ2Eus2oqtyY6JPmGQnUaZ%2FzF2t6ZlnoS1bJ1MEh6MqlgPDdAhLdtmO7KVoC2XDfwdmFp9YEBT&X-Amz-Signature=76ccaf6d82e453a65eec308113a1f1ef276a887f51b8f899a9e8f70d0330fb37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

