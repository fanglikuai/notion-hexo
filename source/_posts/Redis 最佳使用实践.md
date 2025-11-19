---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNRONTI4%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQCzjB8a9umrMest%2FwX%2BXWWU4hEJg2OAUaCXQ9Fd2hBo5wIhAIUyWgZ7VoeOraP111y%2Fhvar4mzinctxTM4k4dKmVjjcKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRM3tmIMm99PQouOIq3AOM58IyY0x%2Be8JdC%2BA3u7yIBtei3uGe8tjAHSEdbLzKUFb0mpY4YpaWWOS6U%2FFPWqatRT9aSR2GfvBu4dnf5XWs0yDs7a1OaKCuyfav9QGwpNgXfz8%2FbKNVvVjNl70%2BdUvHrmDJQ%2BDHlKxzhUdPDrq%2BWbDyF8rkootOnUgHnHrBFBstAdAi%2F7QQrmUv5fy86y4WCNO%2FOw0bS9s3rUJ2geJvRWiEzNCA4gLuAF7EymBzFjeyhzCLvakM0sKBg4aF0FoEEdqM3QB6TfLynartSoQLtnjdhnt0ts%2FM67%2FMPZMXnIhEMsh0nAmO%2BrVWQSAR1%2BWW9F9iBRpH3ZO4b5dJo26LzU0hDmssWYzXsXvPJI9Ggfx12znwpLiQnTC9QrVSEnnyvuuzNQ8VbJ%2FRsmMju3l1cpwkewVkc94b%2BIXnqvZ%2FZnHejGaIdlknIpgRjfZt3HM%2BSaGwHDD8%2Fx7yll%2BP2ZeQL%2B8JDQ3s%2FxTTST7Eg2leuxFb2CZiJzKlmWDC9ZlXQq5r99%2F85AVlAJ1DVRg%2FSCK%2Be6SkuE%2F9lsd9SZz4TFRdYS%2BHnhvDmDehXzOGMhANDQtkfhmGengi6komB%2Be6y8YddDsB1UA1NkiZS7eajiTi8VB4LmYIv6tAHnJYkDCJt%2FTIBjqkAQbvJQaXiKn6LH6aRHZ8qco%2Fsy5UiQDUkD6CJM78Qaie5jyquWKnff9E9%2Bxzuocj%2FqaKT2dp6obZ0VHPQMiRpSWLn9xxyD33Un9fFOIiqzdsP1NDlV85cMxcAVDZi0zC5otqu1j%2FGEcY%2BM4OSZiqirKKzt3BP43PWpxCdueTrjXQmt4Xfz5WUJFLopcer0sTiMeJG94VqtjQl8X67Me4OhsQpYyb&X-Amz-Signature=b74671f79a3a6463e7de61c328c5b16593497102ae4bf0c63ad101260d2b15b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

