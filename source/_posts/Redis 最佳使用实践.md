---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664FVWJGWR%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQDDcOQBoFiRN5v1pMaNDLyyHuniFBO4CmEkwVSYBeApZAIgITNcKedurbhpIXddOLcJtYTscQNKrHzMt0A6WveeixAqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG8DfX%2FJJuE%2F0f7ehircAz8DvwIxCFI53qlFJHbcU70m0BbcxwzR4WYS%2FCiu%2FA2j1neaEoJzKBJ9de%2Fi%2FbaxSOd4SIf51bIjYFlK1Bn9AOHAfV%2B7bekWu6rzzsDdcXdpGJ2vbEYOLTkcbYEAR4lS4zfxq6zsazM6oFa5j1eCNyDeCV6%2B0GT7A1%2B0MkO%2FoYbFTV8cRr9I%2F87UxlTJ0f%2B6uPP61Of1SvFrQGXtgvY6Qg%2FuW5r93OV0aBvOXizqCWPhDccfG%2BwFJ%2F%2FeQou6mCUcWK7TwKLINgDvyeEVn%2BFM2R9ZzOHPEQunI36tp5cV2Wd9Vl%2F1nLluxmj2HuviIuF%2BekJqCDEcCJY80lpNy%2BxVCus9XfNhF2NM0iLAWz%2BaTXeZTjY%2FTvThNXnl%2FRYTCzOoXrYv8N2Kr%2Bn9pm1idAiSJtYh4z%2FG%2BICKBZ7oyFDGHl0FRHpxF8XsvaQf2tO7AAC%2BwxjvnKxLrgAbr1Q9%2Fs2ED6Nqy6JQATKk4lQTLvB6XATGNRbpcSZEQa%2FOpk1EKK6l4AYP4bKrBsK2zfj9pHbxdfg7yq9Ks1I0kg4UhpHXs78uMuTKENcrcoAyR3ag1e7Gz%2BEywuznWsUOXqVMjUkfCZ3vIXFoSus0DMAa5O3HnuE6XX4FuewgBR9y0wh0MPm218cGOqUBrKV84JMlpqW1Nwu3gY3m5XMarOPDAUO9ril1d7aGiZwyVSzDEJ3ZPC%2BrfP4SsJPOw2%2FgyLZfeh3yE18Xl2OuQYfaogipW7lPtsO4qWqRPeAoiisyrQsJkm8nrnwmVbIFKbtbxGQzdWjmBxjkN3%2BiIvxHwGqcDDOW2nMSaNZ4VS5Xvh8VYDg%2FzURuJr9HqhvDTfpZZzpyBfYN7TPbJEQMj3uDApla&X-Amz-Signature=572497f7b80f152348a9ab66ca53d56f42141b85944abb4ff797ce1e8f6b56e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

