---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCPXTPB2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2Bf6ufiP03%2B%2FCTZM%2F01lW%2BtkV4a3Kezj0gBefgdlR%2FhwIgc%2FKYfZr9fuwSEx10h3o3x3TRBRvOdboU80IvzKd2d%2B0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDJSn0ZO71%2Brvlj6rPircA3P2FmGl0XHyPYZFDlxlxt5LjmTpZqSfAq0FtatX3PfT8QiOifNoLCu9Bbb4qECMMRur11ZBBtRDLcj5UJrFZ0%2Be%2BuMZzOSotwqD85uRXtIkbG3E0PVSC9LtZjDqH6Xyxxiu5b%2B2xPnfIGKsNlb3WP9VOhQKsk7hd0FrG1LXKwTCuUBOujkRJTKJ4SWekMVWlug5Wf55dLQgUKtNC%2F7nujBMfvpoNEJvIBDrTGZmsyCONtB5d3Cnj3YEft0vJTkqCwx28x%2BDwycd7ZGbUUVoRdUI8feM%2BU7xtc26nzTIrRdhdEYgSSgeqZBAXJypNUn3Eawm51dsvytGVi6V%2FJce48oiJlM5OrLtPBOoePqS6X6rc6UMSdiWvHR89PsL078enESEuoigh0aLPii1lSYP8SAQVQhigkoFEEMfnmC3YODMzHZheHzY6vsa5%2FoR4729%2FyBZ2QtRzP9fy%2FQtkRk%2BiRl1xzjK0HFqig3MFwwlw27FIoD6OnQSHw6GoouYS6y85GRgg5jF%2FpJ4C7CmUWO1DydWzNCed3MiFvy1PotOuv2OrdGh1cYxHtmsU1EZOX2VeZDG60Es2G7H0K2inW7bllkCs0V%2BS%2FClQ39d8oOku7pD7AlW%2FA12uT637EA%2BMKKc98YGOqUBKWNo2NezRcd7x%2F3l29kLk5IeBz%2B9sqgil9mdHg7lWaYblE6SuorRfirC9cnLsCUp%2FO15cGALZ2G53k4dnToQm%2FL2Y1%2FjKZ%2BoqJKuGCdqlBN3%2BiL5kyWOdPE9aoTF0gExP1nghPmz0b6cyWI6m6rt0VBY6iWn%2BWUQLTEqY%2FEH9Oddva8nPFuNiHh1v7nb%2F4SQwPdq2v%2Bvbgy%2FfNlTvGRuDH3xITlT&X-Amz-Signature=20a1d6ece2b204fa6a7f7d7a49748e101a378bf95616af84e2ceb92404566c60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

