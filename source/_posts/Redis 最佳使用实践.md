---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCUYUZWO%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICic%2FwWBrsqbRavqkyUuUsCLAaRolr4FBuAeflOjoBXVAiASw3kZPcrX91C2tVTbNyhOa9eSoXTWVrC84XBLdRTuhyr%2FAwhmEAAaDDYzNzQyMzE4MzgwNSIMv%2FSzRIdpiWxaVEePKtwDSl4TkrHcYUZXZbByMNh96tB0k%2BzpjnH7vdBn13gXvvzBtSkR7SGHD4hYUbFj8oi6QgwLQOjibiBWYlf45lX1j7xAEdHdeBu8fPi3QFStA2dVlbGcZEhPtCQcFQ4OpmqT3EQ%2B7NurhqBuH%2BuhUKZIDN1l0VKr8R82kpajnE%2BGvmFytsdv17LIfzm7C8QaklN0mKHdNNpmvJL8tP%2Bm577rycOeQcr8dDNblmQwkI6vzjAQ3aztBR3rEEm8Kf16yWw9Xc4oXNsze%2F7e3nwWQh7ecAQDoE5ocODqxuf7Llq5RaQcHkE1To9a1QSwKxOAdyJLzvwXYe0aqlzeq5GhIuIWgQFIYCK1%2Fi02%2BZvCTmCdpDftPtX5iKxvBnLXxqmUHFQ2pv41RYeUZCTAM%2FK5Zb2cSOJplxctP2MvAPoOE1lYdX3gBmY%2FyA9LUNsmHiWHl0qpxQyplwnVFaGCqmTSgJhJqeS3m9F4YxpGRUWqw2NCeZs4HuWXKDJSRmDeLnKjoeDwDiA3c7%2F5k89tX%2F5UNfauBknv%2FfVRWC3j%2F6e%2FjXcgIAydCKm8Nb68q6x3YBAvoC%2Fdrx21ZB8Onupl%2BhDTnms%2BTZdKYrBNJ4DdgbBCO5jqYv54YbWukyjMnu2Zo30w28LRxgY6pgFxyNYZ6QOiIQu4uC1hw86wam9dUXOXx%2Fl0Zhtr8uqpglBV6Yi1FmAZG%2Ft7ekJ5k12lvspxSdlL0WFjpZqBGq2BCIzH5qy7unzPD%2FZawHjkIt6qRgdOaZEM2nFn4r%2Bholq2FsDFWRujjI8sryQct5n3WLYwnmpYBkS%2BZqzfMole5CEWRuu8dwZTFZDWHptvudUh72tRTPFthzVgtoddWj6PL8nTabnA&X-Amz-Signature=9300fc9ed3d1c4f59bef0dc5a9dbe94f663d387b5ead56452e98aac4b5b6ef55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

