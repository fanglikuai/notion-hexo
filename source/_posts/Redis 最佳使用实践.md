---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OCE5DVV%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T080056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQClpqPeSTkWTf81h1YwzM9amJy%2FRt4asf9Mlrv5bD8ZoQIhAPy3Das9UUMgf%2FaUsp0Zfq2YMLSqe1ZKBiIJNTXywx5oKogECNj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHfVlOuIWxVZIGP9Aq3AM3vdpZb7Bx6IikC3%2FM2Xn8RT2AlZzRlnvCrbWuosnbgXUnFyFsHVnUkFjoapbAnnBsLJvoYq5djR8adzs5feyTMIl79%2BpXBA6iRg6hJEf0YlGoGebDH7sXURUt7aPj6TFWsmL82U0GJYO3fH6xdy%2Br2lsmR5Q9oW%2Fo1eCOcjjddMtG7fgNlGIZyQdwvJbYN4K73k%2FKXwLkdeL%2FCV4lJ43tffiqjxg2XaesVjExyJCWvZv8ABSsAFGkxv1d8pMvb29QsBj9uZTGkber%2FKpG9XaGMqrghoe4BRy5nghrKkrSMtgvEgFzE8uZB5KMffTjx%2BC0DGo785S3S6%2FtRrEy%2BNx1KmIKjGK01b8k946qTj7F5tuWpHnN7To9Jtc%2Bhysw762z%2Fyd8T5d%2F4ShTXbuM1xJY2AqHCu2UTkXjwQnwiV4a%2F39TuslVcvINQQ1bMn%2Bog2Ut5m5705TwY1QnixrihmiBNPMgBCGmLK5sGaN4Ymi48eC6gZ7zKruj9CMcRAC%2F00dqqA2WEXvQfxlnLM%2BKKJUqcgwxn09RndEZ1bXm4zpEYF5rDDw53WYhu8XjHjTHKZZ86YhpJVagBfaZ7z7R5Bt4Tyt%2F6326cbnyiSSZ0PNrzwUCq3TbR2C2zV9hVDD%2F0vXIBjqkARnbUGYt98hqB2aM45MubTe5DZ8iuBeVthXCfOhUAemB5QC%2B8z%2FDsERJOZa3SY3gsnH6TYT0pL%2FA8vHYjyiTD5zsYO7cp9qxbcZKZqFTRDUD2uZNXvDIslkVKtmqy0ym7NeIad3Rraa11NUYTSDfdm5%2FBDcXVzd9xJguK0dg%2FDd1qLFqrs3iiYr6Yz2CQby2VF6ohIibg2eQP84Y41LFqB4PClft&X-Amz-Signature=3ed5b016902f645a0cc3aa39987dd19b3b31e259c413f61eca88157837a68340&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

