---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664O4AEQ52%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQCI4bwGC9ULNDmSaorFu9Bpiwf6BiR0N2J0co%2BEfj7YEQIhAOlT6wf1HoFhpN9CbIsJunudLwBOxvaBwLZbOY5OF3GaKv8DCDYQABoMNjM3NDIzMTgzODA1Igz64M%2FNse4MBUjKWFMq3AOxMXn6psZxVkJzxuF4vINzsUi8YzjNqzkckAXL1ycjRuNbkwqcOOO78jL0NZGkp7VjvY4RdhGw139qIbkpvssphTmj3ppXoROizZVyoDGHU7gh50vN1%2Fl041rhcmxFkn2ZuAv8HOnB7CHDJCzI13l4Dtrq08b%2Bx6CqQY%2FKIz%2BuCxVa5KEagWsYAkxAdc0qdPjVQMeef7m%2FIXG7Wdq5A5mPBW5eQfPZmBSJWv4cVS%2FhesgZGC0L3qj7JyTOlPPLpFlLkZQ0wPnh0gnCoDjVyyJ3SlEi5M6wrkLp6gVVLJIeOGq0zGcT%2FGGz0ktsTCAcvRZiauq3rR%2BRGdKWCA9FBwf%2BjaY4JF%2Bwigex1fAwP44cfuVeFzoLwQdB%2Fqi9QQTmOK4aC27NyAsm7ZkEMZaGnTSA7fo%2BkTFKwQA%2FrJJ7tH1SLr6BM2hsvFZp8byVNRV%2FWxqykW6gCRXEsOQP8BS%2FjoHSjBgXBJQlDNjFZPw5TyNjEJ7Fwignm9hvPjIypdrCWTuewbQYCsw56ITM%2B68ai%2F7lqGDAk%2FU1CdyomBr2gVQsQi5grvcZ84Sh88ffQAuq84Rr49XoIAkSz2Dj4QzMnY%2FVBNhwW%2FYoX8rm5GEVdzJWoyRs73lcW9frYkaixTCKgtLIBjqkAcl4gCufrs2VpQ7CBxMgMW9Nb0GfB5hxJpJ%2FFFzrmY8pRsbGspQBc4CewJR79YjVoxttlulRtb7cahR%2FHHXbbffnRnrXAeWaCv9KfTofwG%2BwpI6YKv4RlJ2H7n2xAk4CRnHIIFTI8ObLhodEk%2BP0qz%2B5atEkVy8oAgdqIzT7Wq8cYVy5XTExke4uXDMWfQF9ZDLC2W%2FMivPAxPM%2FCotC4JmeTRdK&X-Amz-Signature=d5f9db5fb85ac3718b997bc77d417996681132509896cbed57580273fd3baf52&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

