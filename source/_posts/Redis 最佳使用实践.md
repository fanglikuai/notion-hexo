---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQTZTIQ%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDk2aTBSTAmECutTU4Cw%2BhJ5ITOoKILCgrZovIK6lC7SAiEAtg6tWSly8HfKCTPqqs%2BJag6K7tQx0f8joteUQcBGbhkq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDJsXHjZ%2B2Zyq6u20IyrcAy3VV8RMR0J7f2gRZjAwHaKjt%2BwNu7IZRwtEUZt3Nwqi5ElFLf48kA5aMIr1UV79oynh053lhryFOMHPrAjkSTkAX4hOm0bPJ6wOQJjRM0aE6BmdNn4L6qNSIz%2BMdlkjwuqJ3iq2jnThXzW1DBptvJGLuXZsTXhPqUBD9sh1nllrcJ30D9YOJ6aZZCSxWJL0uU6L7ifooLERxyrlR7wor44dyg1DOqSbXzFnmnCgDbuAPtHabhhB%2Bw7wndhXB7bTXco8NHlYJ1x8maRXgOn4rE4o6kzFH9Q94URbeb%2F%2BBSljlHhirfQ9%2FiVe4jBtS9PLs5%2F3gJBFYJwYJ%2Bdken1nJ%2BcPDkPD%2F7JilMoL1YzWKOsvKRCejUD1NV70wbziuuEZaBUaC%2FFJ2XgSVS7P9OHgE0U2FF6OJw3QAh%2Baxj5lWvskr38kG33yP3z4shBXZE7eI9x9lb6neJAHD6%2F9L8I6rG6uqtxEGeTLxsTB60E1y1VCyhpUuzMjEySj0tWuSiHQQn%2FJ6CgvWjQGDIqfS9E4bVqtjyeK72CAR8yTLvy16D4vC%2FzrxlXrZm8D6KewOsuVK%2BylhfiwmxwlJTL9hKyPOneu5Nw8iyvNlGJuqEtqQIz9cirNDRvdKTmLIaJZMOCtx8YGOqUBIz5fEcnlSzBUR8GeHVeeQlqL0vKadIWJZMnTkuY2M5%2FGxTHPn1TFttqrqZT0%2BqoJ2zZFRTr1ZTYgruBvwSOPrRtGqykbwRIMTrctU5pXEDMeCqxmLZrt6SxnulqzKPjUcRQjSCaM8OUqrdg5TE0SVRXShj0DO9%2F%2B476jx5bzXgxA0ynPJV0wObAgrylg89NgIHt%2BLkHicvDTLba7A0pmWPXIQbgu&X-Amz-Signature=3d0dba72cc7d4ffc2f565afe7f8201e00d56fd30264828dda9fed26377c12145&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

