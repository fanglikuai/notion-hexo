---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DYZZ3UE%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7Z174uWBP4VvT%2FoFQWRQX1ABlQtMJe5S54WIA6zdJhwIgBGFXVZ4qYPk%2FmPwhgNLhnYovPYWaKbb%2BqKcye1jCsr0q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDAQee9jv7hKNEglj%2BircAywGes%2BhKHcRXyLspR%2FIx2W3Vf8TlgB2vEYfY0MF0e0gj50GOVDZwiyCXxuHfxNgTiC9wfZSBlJcecdPk8Alu1AR2kWA9QuAiDnjGLkv4jPf3WFum2xWbmtkJkfcb01SbhsYKTbc8W6G3fetdwP0R2AAipBBML8m1XEnlol5ULYv40d7PAsrO%2BIHydEPQRMtkKU5RZAjVtAbdD1RC4o6GEfi9SvEuBJ9%2B2RsO5U%2BpDPFD0%2BlOBZo1A3c4E%2BgQBGKQtatBSjTD%2FhGqebkzKWM4Qjwi%2B5de6L5XDw3zt2LpBf6FA%2BQKRSg2JUd8iVx%2B%2BxtecM95DV1KIw2hZwHuzJ8%2Bth2YQumdcRd9fODiYll9um6rKXK8gKS3SNaSIjiGm2J8iApddplLGTybmE%2FHURdkLnb9EiZArxL8RhSYiJK1S9f9z9ALPxpbf8%2FSmWano9hMlE3s0ZtCp2BuxzLyDajY0ZCnY27ed709kYfnau3Et6zwkac9Mi6w5c1yMZR6aQo6grKpzENbu5LzptGK5tACA4ueh6SLuIaM4G8yxwIIw3Wx5vU%2F0HQi04aincpea3czuHylN1wDDdv92WsK2M6xpTyKqXxHkU3gpTlCzoAsrh2nSHAQl3HkUHCAqxXMMyxysYGOqUBMMKkvccFTEJUHiQ%2FjeETAU%2BlH%2FnNxPmZCc0mdY1qeqpaEjdsHmg4ZyV2UZ%2B5mFGDPxEQyfAnYkLQaN2TDy8Bz9whul7I5tkEuyI4anJd3LPhspis%2Bh9rpDsD4p%2F9%2FSYaJ0SNyeygAOarj1HKJUe9cWmfZe6%2FzcvyC1BvAswdYZTnHdkLnHgVvNpu8knbOVZ3vifNkKsoonLjBnhHnMGQzGXRe8tL&X-Amz-Signature=51a1b6f3acc2ed02fd2c88db430bd9f62fecf24bdeb0cc0aa2241fd32c56aeeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

