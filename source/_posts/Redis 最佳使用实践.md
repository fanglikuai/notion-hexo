---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VH4BCBCO%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T150100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCICJrBiz%2Blfj4oID3ymn4glZ7bIYh5o9zaHxeKvS1piyDAiEA9b4hW5TvXTJzuQZ4K%2BGRkn6Bl5uOkrPdt4jW59b7n8YqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK%2BKNZQ%2BpOIVx4TlEyrcA24FGF3OpDbAo3Bok8g56yQb1FD0iQAvKXtvXhLw7eA223RG%2FJwXFoyEsqcn9vaWPd63vYcdGFKgIPkkRjkkdobtHV4fa5aNflCQFz9woVarKAdMztatTvhe3Kg8YI%2BIca8u1k86kRDYI4%2FS7KIhILSAyKTKTJv65HSwH%2FoF1WUU%2FFuE2HwzCaCUjGF%2FukziBNuz4w4GF9eVcEPcKVsm8GYJUFv4Yg57IM5ZfN2VLuKKuq3m33NAAK%2Btn8XTBzoZ7YGieMDes2kmQLubzGIQq8umuJBeHWBLweJgs8MU9QwiPWL%2B2tCWYfAl3PGsxFv5sFQneagLH8B79FBa1xRbC%2FqQtoQdJsXCAsNgZCKOXLUbDDHMr5UajybKqLOVj06led4Z7YCveK8yeVbZRG9PiSSNIACFXfMtkNlf7d5EGVOoiX%2BwF0XANQ1YPFmvQUNCQ%2ByRZo8VZ4nXCk6JcjPQd97MWCk%2B0eK%2B5bMhMf2%2BYY0OYxbpC4WxZP%2FYhvYG9e%2FGFLf5izMQOZckpp7QHN95PtcINr1f4bAJT9i3ySv9xM6kzxM0pCvHLUIg7OCpFfq24yiW9dTF%2B2dYEtiwLntIRnCzG1gDG45mCneuOc82fmY%2BbCMepGQ7H30et7iHMO%2B008cGOqUB5Af%2BmYPIjxY0npj9ztoV0%2BxciIzysqXKMTO818kZycLqvnQK1nLT2sA20a3TaoYXHVtZ92rsf4jdJUETEZCYdmypwurmfwBJ4PUfBrYr2CK9%2FV6hXcEF%2BciOjLTRYUrys33rEJxUaR2UF211TDoXcSzneveA4%2BDeF5UCGEqRQzWBd6Kg7jQsJ7iVJpi17%2B1OFTiWq37qjDAyaLuMCEKrE7aoGTBB&X-Amz-Signature=a157baa16f99c87c05849251609c8b93918f64e0435cb968f59d67acd8969b7d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

