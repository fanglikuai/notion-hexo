---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IOYNLGR%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T150158Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjE8U29xiu7mfofam%2Bpnx9G2NdV7VBapyZQUd3zgggSQIhAPy4wKCqt9wjS%2BgZgzpCvBWkQcDCrGZvuQRX1gdtDOy2Kv8DCFgQABoMNjM3NDIzMTgzODA1Igx0fQkhRQa0W7TEzpEq3AMC5znRTxAmttIh8Q%2BX57d8GqjCoKw1GvFhKUFRzkad70NjsiPCzfGtBWz8OG5ZIAQnRNvklpxTEIP13UgOdQJTxmqK1hX3yYRkB9dFrZm4aJ3xfTEotuK4BMuwAZaHek6D248%2FVIKcHc12drDW3EskxzWUFECsQh8gh%2F9ZEoqdQpbD5Dyhtle5A65yDIgOod65t3WfiO9B3Zr8hiqiJUNC8Io%2F8341LIFZ7GOyszEtz2VaWzLobkmuUqxvN3dSzCZfgprrnrmhycD%2B53Owq8VKzmWfK1CTPqIKOqVXLPTkZjsiG5oakBhm8zYB7QT3EUpJHDtKHZ%2F4aPp2l0QNK4XYxkbRdUnM4HxS91t%2FEZorU6T7Dd6vSwerMupOTD7OfCqhDKXES9GRnpEHmRz6tpwlJ1ZQXYf6opiAgd6LaaiOu2G09OiI946HfPCJEGMExSJvAeTTIpyNublASVTqFLY2UYOyLXXiVvlPYovxk4HVfUArqXMiWXululRp47zD9QybhmWwYRLX7G0wS59EHhLV0vAo7HnIgOCXTH%2BKvDdN31PtsHCkwWC2hzOYkIQQ%2F80TXl17iTprIoBkRj8XD9tY%2FCk6uUXdnzylPxgNw9kC0%2FVhcbbvX3qeqL01iTCa2pHJBjqkAVQrkFX7n5XBezHMweEDlJ4W0QQjY1uT98eZdCTuLO3cpfRTtmm3MVbumPfnzQhveCX50tpM8Ajpdt3gln7MBI84vOpgTDwZ8dbvIyXxdkqe7RWc0t59vAxJQ6qoyBUTL6a2izeQ3E43bARbl5Txzno%2FlG91b0QnfVdmijn5vqAui5D3cowHTpkgQIzuPZ8vnTWabAL2MaVK1mefD8ZQijkQG9a0&X-Amz-Signature=350f8ea700d06aaa1ea3f5c2efaf331e757c61ac715936c12a4b6327441108a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

