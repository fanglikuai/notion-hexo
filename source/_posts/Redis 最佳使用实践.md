---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZH2XGHA%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T090140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCICu7se%2FqKEZGxrZDNnN2RFSEbVkNdlKgGxHh%2BvNcaybRAiBEorfZqLKYdLUdpzRAp7Ac%2BqXNacA9NjhQU2ZEjmngniqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMljaLBpB%2Fq88kseVSKtwDNct9FZxQZWS%2FAC%2BoWeLQTXGy9cJSmxTQE6AOm3rv4m2jc2l22EW0rDCmoRLx6%2B8xDsSqNOtMyVzaJaWrQCVAuXzr1aPvqQd1GV5wmVPHDyknNCoBhfjye%2B%2Fwf3%2FOzopw5cFmk5pdw0wnIZYo%2Fb5ZEMVGGTa4DvlejNs4RSlp9Fv5J2bxkSK2%2FCZdj2HvIjJOQFnp1zEQl2rIpk25quUbHpCjx9VYqwAJicBc1sthDMopT%2FUt%2FkCU5nsKivwIfmdszM8np6el6OmBKuZqNlPfs3P06xnHZlqSNe5QDvDf4hBdRIBZDlk3XQZqLUkBlI4ofMHkGfhszFUUY8ZgD2oLzeGUjmH%2FAsLbHKZFu6tR9H5xxO8U03G%2B6WWGC%2BoNeGNjoZFWURvwBPx82JYDMlaNPZuPGiouFKrc1BNuTYbX%2Bygy5MeFZ7BCDcnDr%2B5lUSAIWt1uk2goaEsQjrce%2F2jGi4JCFnPoxhmHDK7nRhtIJ5640kOvAtt6Q32Ip8%2FgZIk8F%2FkXv%2FTGKbYr5CZpXyoemsdidtaWvI3fykVI8mAlnT2n0orbAknQpmG28kUjeemobBtIlEPC9FFqjqQesFp%2FEwlwH9AWfKHX0Kk76pKBtqxaHWFyBCcVqfl%2B%2Bakwk%2BKnxwY6pgHOUe1g5Osm3%2FfQvWNRCnYyksUDBeZvBCzFW1V2BoVhzBUW8K%2BgX89ZDecV3RJRR6i6zhdA%2F4QLFuYCXAHTGnCoqGBUUHX8tsGsNn9TRZhwb2%2Bj6WXlkN7wtz0nxNq%2BgvyJmAeGww0qpC6HrrGonXSwxWw71qTyfW7uutcplzB7b5Dh4UVlXSP1yalWRpijZF667W8ZebAjADwUhPrJhzvVMbVYHQqE&X-Amz-Signature=c78420a8e018391172ca6b5c97fe16747cfc0f80cf30c6b688e33172895f0a63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

