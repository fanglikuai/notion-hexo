---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6WJKTXK%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIAgVixZ1o%2Fy1mTbpqN%2BZz9xYNOzw6l%2FiAu7Zu9%2BZUxjYAiAFclwlnu7%2BZc%2BgVKRdtqJ90evDN2kt3B2qYCDiG9cMVSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqYvZ1sKOMpkSnCfiKtwDA9%2B4BCA7OimPE1BUnnl8rug9bWMF7i0M%2BEf7HtUXYAIi1U0Wpb%2BjAWndj%2Bn%2FHilJQIV5We9AkdCVfzzpFmBxTiOgVAxrWeFyNKWmnU0gAoLqeToINq2F9%2Fc%2BBbaBr0Ho345NlZP%2BEJxDc%2BaFgAKMRZO9R%2BskCPc3LT18uIac3znPoEMJps94eNXI9Z2aKVECnrOH8eAz%2BlfYSgiBSkPeZ79AOi60q9OnR2XgTZUJVclXjVNhQlPbQDzRrZ37B11hKgqd2YTe5Fn2i1dgWoZ%2Bzu9KTbjVjQ3kHJ7Dk5e7W9wvVBUi3a%2BoPBxyZCT%2BbeaAE4a3EeQWrW3hd%2FTO%2BFlKFNclXN92NqKL%2Bsy8ZWFuCiTBQfnOrOpT6d4sHjIdkAhEa0rGtiRVh1kFAvhTqUQDDyHMQ%2FOK7HRHzoqWXsGyijAIz66mPlJiuMohikrJGRqbXl4rndupBQMStKUkCaoRNKbS%2BzFDMbp4d4WHeLmfR6kQHCc9MEMHWxygwPWfqhl%2BiHaLVNSe1HkYiMMog%2FtWfBriy9FbIi0hwFMOomjcEovrwM%2FOFkm%2FwXniA6tDFLj9iT3NAIqTm%2FA9JcBWhvGGwIARyy9EoykaYvoZF49cC4E4vWbkYmZp1zLiUDAw99n6yAY6pgGyaRIFisf0MLUprWgNpW1P4oU4bLK2eKu5JDtqrHxwMf%2FdhJ7uWueybftDYZJbFlCQLdUvF5MG91ZUvsmPNwYUku8kSDLwXjF3jOhaWOZ2gA57UfcBUg%2BcvQtDQKEru2JyfXoExQjrHdKbOQRRLV9NB5q9FGjlp9fm3h3Guh8DmEFHRjjhJPl64yK3qMZClkcdEUTwZS0Q6RtslrVf3aNQE3tZ9EUh&X-Amz-Signature=eb80de51670715924ad46bac57abb4d5864a874689b2331852396ef4ca2415c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

