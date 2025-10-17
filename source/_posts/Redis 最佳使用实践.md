---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633O5X765%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDmu3lP7QrLWYopeC8cI2JHUPUQ%2Fx%2Ffk8mOfjHL7KUjbQIgZ00JiPgnSxVqoOZ0MZJ7h6vgBWW35lP23kLwsDh2ZQ4qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAiIyf3o00xve2GrlSrcA3vONDuXvKWNmykNqK%2BeEjw5FffHyqM3357aHSRN2sGTRGCEAnRV%2BrZEu6JJcBTe%2Bf34axLeQ6ZkC84SmGX8YLchakgtxeU29Ii02ic2F6n0TGogiY0k2HFDFnw5Z02X%2F2x6ioXCfMAaPBSlR28SJCQPb9aHb5FG%2ByczByLd05xk7eMzTKghR%2FqFVuTqyGVmReKxZ2peyUcb5A40bL70wXkNL2%2BFPjxekk%2Bk7AFCJcKSZwlwL24wLzKY%2FQyc9nvxGr4KSsfkWmKJatNr%2Bct8m8XtpvpVudYy4LWeL9Pt9Qu8tiJgokEX3BPF3ylGk%2FQQTupomsdRvtjsozJTWSchzwlS9b2%2BWAGhXwUZK%2FQ6gzu9%2F4vvYNaYF6a8Iiu1yCXxIpmJrCuVlQDIhBpyTA%2FdB6ilaT1o5dAt0OfHczIH6QQvnDffDtyWSSK9BWxGYYY%2BJO1Z9vXw0wUq036g5%2FJS2K6HAsaMmEtRKhh7GFQk97YB7z4HgEtXML3vTffglI6vBt5fhvxuZAxSE6cjuvK%2Fu5m3nlZhDmDOQMrHXGHYHmR%2BywdPV%2BArAJIHhR4%2BXWWVuQB8wIEVXVf0z5C%2B2LM5kHRK56mJXgHxbP5jpDGyPynzUE3lihmrAiKj2k4pMPOEyccGOqUBu0toGS2rkH8BkpDOYvttJIpx%2FrtPGN%2FYoPy15WvAtnMF19AB%2BjecspUbtjQqgkn6e54xZ3%2BkRPY4KtXy5PXJfjEjgThwbKH8zcz8m7dXS4oC%2FFbsbCu2fJCgn6zzwsLVmt37nRT125y9dUFBm4leAuHp1EB%2FJftf3sT41tA8mffBqjETN7c%2FIYfmZfB7%2FYZig0iiV%2FvcbSD%2BEdcUeQCpW2c907z1&X-Amz-Signature=a7cab730137ce246061897079a8d083e7d69aece5b6ec06294de022da8bb47d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

