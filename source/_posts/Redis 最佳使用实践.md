---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4NGNP3%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCGYrd1NXUdl%2BID11HVzbYrE4Dr5bCbcCYv%2FT9JyGqavwIhAM7Dcg14zMfQG%2B77EBVdCtFuzBeACsLQ8r6zWqs715kLKv8DCGoQABoMNjM3NDIzMTgzODA1Igyql6yfKXXSFv1dWs4q3APS2sanrRIbVSngodVhDllkUnLDbep1Jg7ccdJ4BKbcdbP3zW1kSj%2FJD%2BogJmaFmjs%2FMyxQ3GXnigsEDBno1I8akkHn4WOxtvJ5IC1olDQwHRkfF%2FFpHH7jrZ8%2Fm4hT5aSgIn7%2BG7LEprfaZyg4%2Be6g60I7FjpCRTKsvoUuWm3wkwiorkNwLcOBcUpqb1p63u%2FzPOKAQKaT1D6EPIWFn6NqiKZlLQ7hAKmMZSKZgUc84lYaJivS0svhRbJfFJnt5OyLb%2FxacJszum5WHJtArTziASacOo4ap4PDhSsmlbH%2FKRBMHE2QLk6pyQZdxoGXd3nzLW37UiT9797QpWppamPFgjgMtxyEgljFxuIAmcIIo5AQyoxtinR0iKRB%2BNZ%2Fd2w1aNhe9JNEOiC152Xnd%2FXsAmG%2BPJagtSRAuDNNWiM6G%2FzrgZlIV0cbAWliLuWUoeJ7hdxff5oOPs6BXHxS5x0j6nQXu5B52%2FixO9IIX3xkuWl0zvhYL8kp9WcFPRRXEJfFmtuSEtUVxAx%2BMtvudzMYiref6huIw65%2FSYLvSq0yRbkiREXRdeRQPuFPlx7LCnmzdQd7GmCmHr%2BFp7etsAPFi2eJhtYqFr3uFfHw%2BJnvoSnCNKuN8UgB1xYGlDCrnaXIBjqkAR6wgBa7hR1ZvwrhZznGaHjPLIB%2BwOgU9%2BMDIUV5eryIiM4niWpN8bvAxRz9ZkNSvVPtb6R4vSn%2BfJ78vh15D4w8g5ToXjN%2FDhJ9fQjssTrExVLiSGcP3gRIkK3dzhdRnhV%2BVZA6sZJrINzoCNvB8kmWnnLnCQV7OU%2BwoxnvrJPZm3NPfnnvZz1o0aVFWqayqRvXg9yeLt88LfuohBOcR9E%2BP1bw&X-Amz-Signature=802d329e8390dd4c8102d2673a5029db6952f11554491a668368817dd562b14d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

