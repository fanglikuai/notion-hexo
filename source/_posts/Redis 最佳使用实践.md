---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6C4SVRC%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDL509gmZMN%2FfrVWLyjz7xoHcuxf0yII0s1wPGDhfb%2FAgIgFCozIdRR4wtmoXH3Xr8pE43Bi6ehCAas0V4j4%2FVgFz0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDCeTsjOyKoXGtFta%2FCrcAxwzfZKvVelxVj%2FZssLHlxGT5%2FaTikCt6mAYcSzZXB7lbA18GV4zip9x4OgRaaY7BxO76jhV6y3dS7Iy9IlE47D9vLqxcXm5F0O8jmcphDi0TgBI6vJu%2F1mpyekA4ONX0kl34oKjQNXGw2l1dJi5AiRjPqUBvEbAcIqs4MLxy9Z0xaacbN%2FQ2SX%2FIvgdTbN91cL%2BsMPMQIMPJ9A%2B%2Fj6F%2FXdpQpTI%2BF%2FSFvh04DUAv2fzVteHy%2Ba%2BCOAqizwrZkTzLtvPSL8lluGtZFjohT21KssUOyEfb8VFL8V0eZ47DWi27cYL%2BY0GhVLbIMuR4IdHo4NHm71NrvfOlwoWWGRtLFcCtVtwiFOwEZoBCHqXWlB%2F%2FbohHdZXGkM6C714hrMPIFK5Lt6pz863DlmNBc8NTCBods4hCmpapUjx78DIXCPq7UPM8T57HFhn8tH9uiYg5d4njASoQ9TpHMxogfeAf3lSrH0AR0eGt0pcJG%2FmQD2DetYyXt%2BHn5ax1AUBqxDTvxhsEgC9Gp7J%2BZc62SK8zVRR2%2BPaU6aB%2F2owlMmT5jqRVu%2BOzEzyPB9p4Rlp19uAzeY25wr3pOuf2zM9Jo0N%2BAZFwKIqJ%2FylIXQ1xTRn43R%2BWIQ3yMYDOtzn0D26MM%2BwwsYGOqUBsKoJwfy0vayF2r2DLlToK7OmUxMLad65waETs2CGr6LmZu5TEEruDE0aR2eS88ZugQhsLtq4rqnVle8kl%2F4uSoYE7daCYlVkLcjZ2xU9F8e%2FdY7QPHBvRogDxR79pjIGi3o3LpNcoeLoR56wuvE%2BhGJWEveV5l8Kmn0wh8K343KeIIx6eq8uwymHHzW5M2JomOQ3%2BAT7eypy7wBdblCk78KgCpYb&X-Amz-Signature=2b4be73089539f363842af0a6e775d1ac3727fe4e532a4b1dea4a96b48a88de0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

