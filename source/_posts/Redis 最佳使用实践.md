---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U56X22FW%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGsnZornaDvtYZoOzn94XQ%2FAqU3gFKc5hggj0c9oNN4QAiEA%2FfC6uRE9GI%2FfhAW5q%2F4Tu7i7sZNx1xpXtsrObG1xQ9wqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKWCRHrooGfFeSlZ7CrcA1rk%2BUpD1Yv7hRruOXZBA3joRumhWH0j1K6hu0ph%2FOARUxW5cH92fOn8ypCIHH0tCWc0EbFSYZv%2Bq4qpxCprojT72jg2BGAc2uIQCsQN9dIfUwZoYaNpA8bj8VGmGw3nd4%2Fx01%2FEV8Q7n3aBm%2F0OWwTqJkCXt02x%2BJAtArWZ2BY39kMV8NV9gPH4Bj5gV4gb56E1rGQPqu5KpIniO0ibY1FLBsXkCTg%2BVSDlvZLfHhPIL1V0gduMickwCtjWA7u0e9qouKSEr8OV2IdL8QHT7ZQnqP%2FYk%2Bh%2BVpQAAQboZ9v905a1Btaw3wZabDd6YL8ZKs8OHKR5g1JRlVjIGGfs6x2mI91eb6NNLaOARf2Uc1xvEqu4FAXIO4jtb1lPrQ3gDKd0TiiBzf8ZmEBzqx7yXMujL1YkZRN3Dzzu1QpUxMqJbYBVYTb1DfrFVyptdk0PK3q9bw3njSf1vfcWqfkFq287WiRlSF%2FaD05iVyR0zFZD%2FihbQLQIy%2B1xHYRNKf1XlrnRxclQdzU8Dri8NR7PDqzTRDMrnXrUBQ77A%2FKdPNGP22kZF04v0u4Drxz9OIiujYBcSRh5qFTwFd%2BehgRDb%2B0HVP7MwmR%2BNfI%2FUuAnpu1jDgzKzPgVtbe1D16wMO3c7sgGOqUBcMnaKs%2BaOjWASNp7reLBLQ9zlvGKgdUlF9J4tUfF5WPpmSMHcGKmiWhrMQi2gXK3EnPzLq78i3ERy0XnvlAKxQaqb5IFGxD75D1kteuBQpthQ5BBn3TviSMf5CaghMDsYSyZdxt4LOD3dLahTYc9xs1fitq58whBEcZMkmNIaItmYYfseNTjYIuRCZoOzno6TQJWwljiylRia2JsmlTjaCMKNiCs&X-Amz-Signature=d0dc7bc736d7a02d3789fe5035f90031fe0fc708f3dd4d84cf993dd946fa0071&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

