---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNNB24L6%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh4iLjdZsgRMm4fBB0YgIh4SWTjZhQlOFjr3N43FC7EgIhANlwG6%2Bs8AwJ%2FsmGy9gy2M6c4fVpxBa%2F%2Fust7conLMvGKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxOMFy2wJeaHmLZ4E4q3AOGRkcS2k5FZi41ygPHffSQOTP0c7t%2F9jXzFVfMYkN4evFVjOL1V%2BK%2BJK7LM14rGDntamdhJcQMgeTVsjZ2bS7UK23O7GdsRoP6O2iMBfq8ZaRf0Y5WfSGjBB5Vuf%2BhUwa8%2F6oZM5pE3FCP0eeOpPv6VC7u3IotP%2BBCw1uacBR2x5ZuaiT330FcC33PmK6IPorT3c6PZaJhrSSIqCjFud%2B8pBLxYXvdvSkZlRbPHZ9hsmm%2BWj%2F9SSS3DgaSiwPtbqhl6D5gI6Mz6h1Fh%2F2jIWl0e2VJswcibq5CzKTuoI0iVmiPKajpUODn3bsA2wpympD%2Bq67kQPtmOkUjtEczh4Lr7UIBeCq%2FA1X5lEiKYSYC22dRqHIAlD4nYWPmwteKSRqp6j4teKcm%2FgHSydxwV%2B6MynMduScKViURZxAVWAQmR70MU8xwMTiTSW7d9YM9UPTly7xeOPO3xiAX7ldn9KBCpHFzWei7c9APiNEmZG602z%2Bzwbw%2FYsMi1TeJt5AdgxAHMWvTT%2F4xvedlSxx1VeMq2D6zMHoO6GFmt01sVJr4YJgk7Ep8DhoGYcacO9EXYwv0mS6eCfFGzoIWbyP%2Bne0LSwA3HzXD0vjyOACr9WElUYpquJrpu2xN9UMusTDuq5DHBjqkAWJOH37pMELjJnANrOdtPvDvqmuh%2BbbSxFUVY8GTWY0lCR7vKB0q4szzHOm5JChmFwRloJ2mL6gjeH9UJlZW6eMsDi7iRt3WQf7WtvL%2FqvWizlpXZHm%2B4Bs6pLyWmb3JCW6QKRW9GYXxKxlZXaEMp%2BT1vzQzGzL4MvX3Lif071c5SZA6SFprGEyq0Qs8DyvMneIg03lrkzS0eTObdeet4i40v6ab&X-Amz-Signature=82b569318f9e44542d17a81f6a195bafc7d422703809c3a05d6128811d1ff729&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

