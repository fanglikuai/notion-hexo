---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HWMXX4T%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCZq56M8IyIRu6rvIWM0eMRxnn33edh79AwQrabVV5UVgIgDBKDE8SlG4XvM4plcYbAoEzoFj0DpISi0IKgjO6AzLUq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDIsUzF2Lc6uENZV2jircA87gW1qQbfxT5QedYLkmO4jB2fwBaArBiQ%2BqZA%2FDUAQpwNAiOT7H1EeinE72v643%2B0VcQXETEEV61nM9DQt4AGx9hvyXpYaYh7MlluztRDMyv0p2eD%2BaCmBUOxtVDIEyBe8uvbjWzITgL0mGv1ESks68pS0TDKpCEp3SIq1S%2F2iMLLH76lGbxZJwk4vcjTsSoPnl5T5Vd9PDQOtqfebvvpmlYRPLWjVi3ncgFGZrp%2F6j52sqkzW4ScXX%2BYug3LhBHs99nxwaML%2FSNrgTqqbSVYhSkJ3Ef0%2BJXPetMNLtmCW4Om4UDulsqVgcEp%2F%2B%2B06GU7DOnzPqjIoK%2BKjgaZQ4KJsPOafatEuM89f1aRrJFCkTbbglc4oLZQiD9tYxo2F0lh9CZIRI3GeGHn7fjX8kGhiVkMfQe4Y06K7OcRGMP7RBEtjEwR18Ifb6wzYo2UhGOu2si0VBGbSlJjajIBOLrX9F%2FUTJtcoY0ly0ynHQQyh7dDBApUFXOCRMPkl23nfYt4m7IbB1FYEIP69HKFsINQqQYjuW%2Fj3b%2BipvBPxlR%2FaNBEm0TRhp5JEQPEbZ80fvrZN%2ByIcBBGMb3uTJhfL0IkpD2AB7jyuwWkr5pg3LZRStdefVR2xn6%2BFHMVC8MKSZv8YGOqUBvV8Dzydmvgh8NUUXQI9W31v3it4WggPHmOmKsp5AVmraYEhJ7JB6ccfAldt9g4GO0Es5nRLwH9pi3pSK7Uck%2Bl57bm9T2Uz%2BFWSC20zRYsr54OZENFKnPm1fgW6gC2xsU4zII2rtSdFnmGHmheBcL4iCqvbO0XXcQSD0pP9P%2B1DSEuJEQAcuMcjKGfLEU12%2Biu0sGQEWw0LixqanjFkTZmugoLMG&X-Amz-Signature=d88b3a82f5858d500e58dec89e09f90aa9292a6399bd386a6db63d4f87318dfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

