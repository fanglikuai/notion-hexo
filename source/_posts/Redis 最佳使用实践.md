---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QASERU2Q%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAx0gNpaOqTTbtb%2Bev1beewfJp5u3Mhv3uDDifXsrNGeAiAXHP1YeImz8WZG%2FgCCpFP%2F8e8KlZYpi8zq7SLNSQh1yyr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMDtYTI0yGHjmCDgrtKtwDcCWDQEBKRzE43MNpaq4Dr6oAp1RQni5MpTBdHK50UWNJ%2FnB525%2BSzZY%2FOBo2hO7qr32zggThjLbTr2vbFruafbdEg%2B5NnV81Xh83dQutmpDxvv1Ac2KuP8EtCrniw49kJWvsjrTbsiwjR24oRXFP%2Fe2KOhcs%2FJi%2BNB7y1Uzuu6oQdlPTliCP0Eko7G%2BCmFKc%2FqKyWVvoYENI2C1oxoG0Ru0H6LN59T0itOq29Burt4nlbNFzYm2tjHLtU1AuaKtKa8vKPjEEjQTTxYt5yq8G367k7AWyHRbIDssH9Pn4KhXcyKv86DrrCPdxSJhgiB4TYtsaVmhhWraO8G5ekcUf2LCcA4f3pXGmie2D%2FrgdRLIjZsNO3ao43hOkMuouX11SHqY%2BDpei9LqtuaJ27QYxRrBgrYfmodiU6BPdkrWl6Y8IAb2Ct5CaNI%2Ft6oFYgSZT%2FThFCCSf9BH9chErRkVlcPly5dUmEG0TJ3SNen1c%2BlBu1wPZIWmR7YcjY3vKKgIBIJNYgDwz7s4B9zm7d0%2F0XlAc%2F9Zg2HyE5rgfWVaW7Ius6cWzuwiIk0uqfmI8730XtwDUUt5XNs7hpGMYMqKHvSNeHnrUN55CG5lKMsBKiuQu1IaKGCjc0lGTgaIwi4m8xwY6pgE9Ue29pYdhDAyyhGRhDmajntdAw5VGwA1kly9KKmH7JWayzEgmwhq7CJa9SUf4N6gLtA33l4HZF3mWcWx8877U2U3HCU4V573FnCCRd7x2wUXLf5rVH9zIicfzAVS7i9VLdATrwg%2BKU92jO8kczpmb37e%2BWhwuJpkBZHhFJyDwz0OL83XS6rON2GXEDPnNMDeU3LpZSIDDiSuP8QxihteMJLbIMboe&X-Amz-Signature=aa46b070ddebc85f6f0970f1520c627b0b1af927213345ef34ff33aefe6a7fc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

