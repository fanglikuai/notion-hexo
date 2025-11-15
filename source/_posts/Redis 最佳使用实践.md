---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDUTSPSW%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUpEbhYF0ccEWcZFrnbK1LjbGB0yr6%2B%2BzCjj0f16t5OgIhALxpDCUEhNhm%2F7I3xFp4xX7YD8WW3dXK5yzPyT7q22rjKv8DCHgQABoMNjM3NDIzMTgzODA1IgzrxdfO6QoLOCl1IRsq3AMdwVWVSw0QWa%2F8UVENul3M%2Bsl03EcME3o8jMaGhT0%2BWspfsaz1AJJilIOleRDbz3tHGxu2fbXDCt5yvSn6yGbGExZdsudQEpGfg2bH2FDlbDccyY3xM%2BGK1QnSeJjXqF449tLXazrY8vVFtoROdKaOU7oY%2BePOA1mb9lc1huauesu7I1l%2BN%2FKldqiUfdyWnUDwEbVEhJgDkoO15F5Ixb7VONXSTWssYKeBpIVsOYdN1fCqOZZFGerJeNEwnjNoFg2%2Fk%2Ba9D1S1PQUok7qwcvwwZv3ITx9p5SKnLju8a3xOXSnOI6d0YHgXzyOEEEQwqoudQyR%2F4hove42ZBuECqWSVg3P63HYvg5m93aNhL9OCK8WBiENOHYMnVh5ymahwKB0a8c1qwZggtSoO63HHLNYL1BF%2BGueHpjqOYySJTYsBcBikuVuACyhq1XJ6Uvy9LYe4VXTKRArsWP0vOyw3gDDeVZZbShEflqitqxnpP6IfpM1dokL%2FPyNrZRewJ1V00FGGhq3HFMV3FpOlDjdxj2gmSix43W3W13%2F2YxYaoQWKUCXviQzMpQZzeUlpZ5li5G14attCi15RT7NLssuHWY61dHWJkIMa9ld8NcFPdrKS3%2FQ7qXpPpzBklmujojD9vuDIBjqkAT%2FhFdAV7eFSmW9ev8cpjONK4CthvGJEM6Sicz2N5TVtLtzx3qKt1WUrFFDLeYSomod5EmgkxrO1u5ADKh%2BWEyXnslBaGLulffIWEp7%2FAiB74Q1APwkSkpcPfqghMfh2SmluOV4JzJBGpTbKMXxhq7zHVJRuB2BDrkPEIBaodLk83KYgP8otwj85%2F5Rrh79Pjwmp2f8xfkpUbZSK5lPxruakH23r&X-Amz-Signature=3fe3bbbdd29930da14f4ed17b384fad5f34ce7f364845c545c69428883baafed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

