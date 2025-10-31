---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGQIALZM%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQDW3%2F7SmNiJ6zMG0BhqXEs9721zboNmbeSIBM9rTYe9sgIhAPAOghTHKIVoZHQN4NcZWcl%2FVWXsUQiucQjEf2eqj9EIKv8DCB4QABoMNjM3NDIzMTgzODA1Igx8HjROX4CWXQMnEHwq3AOIoMIvnZemNmFMr3H6CJUVnw%2BPrYef5e3KaBIbSKlA1rhk18uDc9Tg67OS895DfcqVIiODhepSq8VSbW4B95WKzW4jQ1DkhoQAV4SUq15PBIdBCeZq%2BTyMqDfllnymoGH3Dv4HMSCLDEUQad4VqY9eEr8bEfTmDholh6P%2Fgr51%2B3TTp%2FME8Fq18B3HLOOpZXI04ISzP1yUr9YQCvzc6eVowcW0BgekEqoGYRniPFewlTZfKgOWCdreLr5u37IpHdhzrl%2Bx0w%2BRm5X35htKzWX%2BExDhwjDFqQLU3nCkzzGn4%2FYQZlvB2T%2BeZbrbQJRguaTRVubXtzuFxh6CSJ7I370cy6MRAYU%2F%2FrPYQmW4%2B6xjlgEVpB%2FD5K6CoOjZCerGEHE54ldM1gWrW4hfAvNdDCc5tRsJm8ufA0%2FBrxQZZ51xXOmh66kpMhnCZrTREAiJkiGuf%2BhgBgA7CBzZX42tj33AWk3%2Bvlb1vgO64fbjCQO0qbxjeKbYwciLyK6ugBy6S2RJW%2B5ubI8yRzs8IO4nzn50KZScTqJgDZ5Ldza7FzVielmwan5jliKCEPvgK4ugx2vYV3tfGp%2BcoF3BSCWRLF2mbjjBvPO7eWNcbXoyGKwgmeg18GaAv1zrM3KtRDCIwZTIBjqkAedsqJtbIQq9Nje4qnRcs2%2FTsgEp80i9cyJvIHFE006LqL60DBI1KC0XZnjnbNdS25nQSqxfVeOsdJ3SMxqD1uIQKoAIgXEReXhT33gp8v%2FaZRNqxmCSgUWnx5x2AVATpEA47I5CcC%2BfOOYjNnPsRQbDeMeXXg2hh5a6cpmc8zo%2FOdfRpRfSVzqcg54PUo6%2BOKdtgHEdY7Ge0mWniYBFNmWrMGQ2&X-Amz-Signature=35b0e9553d06bd5b15832f9bbf37af5349f4601d1f2dede6e41af98bca48c6bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

