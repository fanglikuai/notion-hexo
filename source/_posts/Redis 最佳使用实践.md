---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOQVNYE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKVMwOUQn3Zm6SOCfy2RN%2FwzCK8h46HVYi07yascdFewIgcOZPjV8d%2FfWm6oVgnOn4l2iiuz5Wgi3BbPElLCy6%2BgMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDNNn%2FABtxhOeZzxUGyrcA4LJ1ToErrABMQScaIfbGqEUV%2FEMZR%2F87%2B5Gdq3jApjaXleqy84E1NReN5xKKuhg%2Bj8ZJXmYf8B0mjELRz5QOg%2F7fteT5lFwLXTYHedCbHXZdRpefdQP1t%2FgudJZ3MN%2BmVCxlEHIxEfAGx%2B6Q0OaCM3ZDJI5zm%2BrtYxZPGIgqLaOA1rHEULlBtEBH8ByIQaBRBREiVehOibRdg5xn2NsWNDFRqd0qbfuTGiLFIW84%2F8ourO5Q4g%2B9RNf5p%2FFxoYzfCRWLOWAeeJKz56pgSpu5AVmDjk7RBZxwF5b6G0doeooytIrZHcMPt6UNq4O69jIXXot16Om6Ki9kJPu%2B2c1hW6S88sU%2B%2Fc7kuWXHMZWldBJNDJHGh%2F2jUj8vZf5fRvM7b1%2BnuXHWFRA4iIizXGZ4DaZltO%2FUoB8IU0C5pS3Y%2BQFRl5F1JU%2FAfvFw51GTFqGy64BqklChr73JU3AR0LdOaPaR5Ag9xX7qxNFoqPZ3KwRnufP%2FOIM90v%2FujsnPDoeTINFsBEHSignSKGxV%2BMI1Xej2jQ6rAm1Z8Dav%2F7DKjWzNu1BFtNk%2BYzI2ZBMypeZi9ct%2B8mX9h407dRBDbS%2BXHfpeeY7jv4R2gsVgUOhPWWWK0bOJZb2WOXPemghMK%2BNxcYGOqUB5ni%2BxGK8K3Oz64u4tVTkuaQOYR4tcYKkz%2BluXEEbwTYr5QPSsJa9tE6%2FQgzDC470NJOjpNA4mgu8kjBfpNPDCBF1szB2uMwKREaJVONsEk4hYUa5UZnl8UdO3y5lf672lYF8eiDXBs2EwyI3HmFLp9YTeeCAcXGLU2ml884WD7nNRiVGmy2vWapaEpeBbgwe004%2Bnh7aD2YJpt6a3j4y0dknvEoM&X-Amz-Signature=6cff6b53d6535e331b048db4ee6ac0ee05309b808765b7630708c8929cc9a44c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

