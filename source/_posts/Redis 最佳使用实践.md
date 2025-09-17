---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CLSP523%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIHouFWs7umBK63iXbBqg%2B5KvlSkZGpbfkZ1MmSU9olJZAiB5I0JZsldDEzFfizhYkWr%2Fth%2F6gNwpSKIjiS0sjL9Y%2FSqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrVcCzYHlsNRILIzuKtwD07GN%2BukEjFY0toM%2BLoZ7xOyKSyV3nvbxaKky%2Be1gDfEs4ZEknitgDhtuA7tE%2F0rzNUP69ctUOQxfWl%2Bkib4qvdga66Cjk5rhGPo4Db3AYgufI3pMkzMwilvrRQUQuewJFe4DG%2B9LRQji23BYt%2FwoNh%2Bogwg61EYH0VkQ9XtgpO8n1oJUJom4%2F84mhflS3vzsdvgf24nhhOKmg%2F50FYxoDPfVPwlSFtsYTF%2FGy7COzB1BUjlpz%2FbPheWTv3wUFnJhWrtAMJXpOBGMzlPdyzr91bodcIbOEY%2F0LcFFh9w1ePFdwJJQSwMmBfxJULp1jOsKUiEllWq%2BRhPSifEwICi%2BbY7yC%2BrYrtWWDTmaXbhBThXvgXscQdJAVVLII0RL0bLpm6jALQli5NSjx2a9d1sY8AsAjU%2BJ6tbB1dA%2FOL7J0l6cGQX81WcRtyoSWkRqgKmJM422aDnrScqP%2BwmHF19a%2FuVFcdyucKqYdANVpDCnYUbb7cOQ5Ibwl8lpJZ4WVvCNfqg8CtsaMQe0cMrg%2F3%2FOJpvcFjN6AHe90DZaAvdGYF7y9lI1qFZGx5zGQwTbNH%2BhIxZjHwWP6ZeTVjKxLuhS6O5Hev39R%2BlV%2FaQYIrLXsMlbuSobV8FjLh7qd8kws9ynxgY6pgEWRBPQy9jUlZI1F2x4yyJD6xR%2FjdcJRFrZat1GUeZ7gr60aMP4NkQFdqKRQYXGpttzJJWkjkamvWF%2FyQD2q2u32hAQJgpLJKgt3wQDY1fyrcF7lQmykvDe%2F3U%2FeieKFvLO9h6JFgL9Wsd2YyoVH0VmOFPG0%2BeWOrrEQ7EDVUmmfAGKhxpgVTWGushPFg59l5yw3UvJe81RWFKy7oMDqMpW0nTa0I49&X-Amz-Signature=8beea0815f8ff0d8f03a1b305ddfc4049a09d6753c7c0ab922814fe60e675699&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

