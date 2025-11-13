---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DBIVIOE%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbmphMPNSBqYUlmC0HbrwLcQG3YfHjICm3WTWRErn5IAiEAlWgY71Ao7flrmiLslSdbVXeb9BobJ1kl5iQSdcM%2F4EUq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDM3Z9%2BSj4v2ajNFVyyrcAyx1CS0qR8h4v4GgDttPwx41PumZ7LmrCAL6LKAHmLyMRCIkhR0D4kTTUR0iXBYKTy5iBrLevWHXxYQ1oUVPpTIfNQ7SU3V%2BXDgNqRBKc4HOTQp8gSGLIL3OwvF%2FIeqiSxi9jlz54nOAE8NlAQTIq7mLaeAc1NZoJKX9ukFvNyft6t3amesF%2BIeCxag8L%2BHdDM2hyNvQd14rLTALxTbe75h1RwtT5R9%2F0Nmj9AWYjmO2gzCGgVcDx2LbOM8Yg4YuMiPoSiVKw%2FR%2BHDBQZGaBugh%2FELchyvANBnhNclrM%2FyX%2BRba3IHgiyIlhYunLCOgJGiKastV%2BknJaSZiqUp%2FBiQYMT0F%2Fltq7wXg92v8k3eVGDwzAkkBVa5yEAs%2FtFmgrQdUpj5Ye7ePtUADmqd8RDskBhR7VoURKOR%2B38aR%2FEywDShkogjTfXMaoSW8yfPkRrJmgd%2FObmQG83Z5AAWXbmfHPOtLQj%2BIwL0pxeAd%2B%2BOuehY6WT9f4ZD1UgEM%2BOGh6HXExaKp9%2BY1zkR%2BGVzCeQuwflBdVFh1ycofHmYar4B8iDBrxxv6VzxeWKVgDqDzItZk2TD1A%2B5%2B61GMDxn0ggD1MSDrbfy%2BUJ7BCEelIk2aBD5UP565IU2MVUUJSMLrf1sgGOqUB9nEg77SbeQg%2BhCX0amcHERsUyzkt5%2Fq40jnrwRjRbTmWEuMx%2Fsbvgv8xnqo3mBMqvNlc0kxa8CozAGs%2FkE8y0VDGbUUgxiJgzoXfuSaJNT0WQ1t%2B%2FFe597HhX9U6bxwU5m8rK%2BuZNMjnt6OJh%2FxzCtALNdlCnehigYWiAtw1W93BPqry9tHh%2FZHyEDk%2BcQAZ7m4KNlKcJXKHxQ7JF6VXFxEnHchG&X-Amz-Signature=bc0c1d1376c2115e701fa152853a51355fcea63f3b5869a6f0dab7d8b1c61146&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

