---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666AVW4OHE%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T070100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDv2%2Fyek8DON%2BN6J5%2Fz1bXEKu1V7OQ5UOPXSt56VyIGmwIgCf1wWVfwc%2F%2FxAdkaYFIVJ9C9pLTOubArYt309khUoKEqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOSVhYxWJU6q1Fz5FircA53hvJm0RZ%2Fa%2FUT3JCPLtfhapYLjalOsqDKeB8FwrD6MgUmgbls3Ng5MHaAdYALm4nrI%2BNjRRzDS4q7KDYlrE1Q9hmbGJXxEdQrZvsT5tyLwTSPCeSqa%2FlNxv%2B6LnzHQ98RJ8%2B0rbuAjd5ipgc7evZWxJG8owJ83s5A0LGA7snxIHJ7sCqrcKinJ6P2O6CLHvuyjPYBBwp295zOtpGZaC7BY9YWPTYTMZlQGitp9fxAbnIqAn2UeEDFkwbKhM4Ej37iAKlWGW8xtXFLpTnY6v2W7z0HRlIhtTQlflmSIFuQqPpIak%2F%2B%2Fe0xu1YP0XElr9v7NOdEA6TMCglYZUVWVXUTYiM7Pmo9FMDTzIT7KGQCsaFzEsbVsxbkEkJB%2FciVgXOaRDjjwLwH%2B02cEFXKN7mAXYtcLmj3gOC%2BAVZm95Ok16CLzu3vXVFwU0kyPQTXvxwVze93JtXxTNCOx%2B8z37H6cVWEVtF4s1zXeJAGOhNaQtWvGAi8nDlXmyU0tFXKvrp2tQilTJW%2BlO%2F9tac86nin6Rh8mLNbvf4Bmeb2h0fvpCZGavhNrkoou1beP%2FxSvEueKCMCRtm2sL9WkA6yTsCmRRLlvKWc2jONOOFW%2FSMnr1i90W7rELFxO3gnRMIDaoscGOqUBR7ZygwP3Alq1FacTXD8%2BzRFPRP28ahA6PqBCQjiIQMAN8yKCgJ1Gk523onbhd3GaR0Qw18pNGbr9zo9kRh2BfuJi5gNsLvUyDM%2B8Hmb4860BzYBMHlIbCZRzUtxFp4cz64umEtqfZks9iFHsrruqjnQ%2FMZBFy0brQhjdsKo8KZo%2BgHK2aT5tgJrzGhnmbF8Bg5c6jWgI8wO55QX74y7aW4niBbhs&X-Amz-Signature=eb52f0bc194c2e1dbd838ce2c7411f4ff05b69c1e1c3b71335691d0f73010cc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

