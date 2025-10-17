---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVE3HNC3%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJIMEYCIQDCjMaw5ZJ5VhR%2FMvNhrIcttAs%2BGBpbNxyJu0%2BklS8XxgIhAKrSSqxSgxMumK6T4BtFJr6CxqkoU%2FpYTu6e%2BeegU%2F9rKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxywfZ8hpVNhDFjnhwq3AMBcHPXxAaMPmLrUVqg8wbit4%2F5e7A%2BrEf2wMmD9CmnmpWQvjoZcj%2BzJyE%2FNG50vZWYrSY%2BRsb3wf9wNV8vWuPVTCcyjuQtnHhocKIopKkuWviDBxvUyB%2B3jYbmWfh5tw3spBQeuOV7H6uvzx5dvPN3WsAuq7pLCXqJEGJ0KHAXA5WpzeWd%2FuJkSL%2FWDZ5Amdfz3ClNgs98YXRfZRAdFkcWTDdw%2B7CWDf%2F7UxVNJ3J0UdW%2FHK6yMqyTohHNNtsWVEfEy0SkkNiRI6YmHbEurRkFcb2woVTGwShY5Rj3zXJJCCm6pBE8BIibzwfi3xECSrHbjxpK9V93AA9qMBlsEE2l1nWxcfDNbv5a1hezG7riabSAWTaxpnJB%2F2SoWtP%2FZlhdmCE90t0%2F8Wj2cr3CiPkyeSUV1PsZfNU1QLi9UVtXt4UOI%2BEfS5DffPSX9KNLneAM93OYa0s65sEzCvG8KHWA2zGHHQWyJXXKkAJRMnDuBn6mkliaS4%2B%2BPN2nzU99imYHzmibTAzofyM1Q2Qzk1ay0ZCbOl6cGMKWN%2FZ47XnSFOyGEswAzfCbN0l6MW9Y4My%2F3qZozzTslrFcmHtGTWKARkjzGcQL5zgjJYseSSCzOflW2IMoSPJJFpHVBzDl18rHBjqkAemafd4vNamiPhbDUVPEbIRGfASuTL98RYWqkUaVtCWJ%2F9UdVBt%2FBHkGZbvX6NdNt1WqgoawE8LBT45dw9r5WuUFI%2F1fwXJsb0Y%2FDOafhe1sIDkvHf%2BN6ZI7vrfeqKTzuFeac3Qn79WS51lpgBNYS4miVklGko8DrnF2mdDQzjJRJSH9vvIuJIXIp1fiqCbP%2BZozXvMM3WNbuu%2BPcUTZTD9QrVAZ&X-Amz-Signature=b0d97cfc9dd16412a0cd8a7307cbb146930c75dc6de04c9382351a802dfefcaa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

