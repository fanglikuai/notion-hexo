---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EE52REB%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIFaErhx4Yf8nZrhUtoFHtxdiq58TTDkEVlRjrvzEu9JGAiBQ4NC5w6Px9Qi7Y43VskLMtH12w1gHA16IGdRtazMQECqIBAj3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3T83upY0x9fYejfNKtwDh5X1bkJkkTa419Ckkvp97orXW2lPLAhsb%2BVraM1muWsFRpLiEzDLOnsd%2FGWoP14QOoWzW8kVJ8nEQ2ZeYKMCeLNPzDd7h3M8Y8zhjVfE9aBUjytGMoYgvmR%2FoHVa1%2F2KCQwZUTVpD7LFMx3sQ7uPfMyYnM2zvxO5z050sB8M6l9FP4MdJrqTZp2h3nUieX1zoCHEzw5YB1dxrd%2BkkNIH2KZUXD8xZWaLlVEZ8OmEzZtgQiKlBzaptYSL%2F4a%2BIbAX4HtjSNsrK3rs1YjJ%2FeEJCn0H8YC9LQIhkigupAmFaF%2BBOmdSuCgIgqAHt7gSMMhxO919cK38sbOyri%2FmlN3Lv3HCZ32eAd69V4I5kSTLQi5pdvWSZBoyVyF4TsxRvFOPtVw78FyufYgOInQL7WES0oGW127rc1K7zpD9Chg3Ixq0TyxnO3%2FCYlsLNJ5tyL0jRgRhTkEBidf0DMZxkZwrXSzhKP0bJwXQcAMcfQD8VlYefCwXlh6aEcy%2BSYqSKKm%2BW1uTFC1fICS58h0OoSiPEiAuBJU9%2BZaPrsW5P92U8o8WvDv7GZx8yaLUuGsO2KgOHtM0erAdT897xSn6s3GOTYt5d6TRaG2LpQbzNCCUTdvfIULQnzE4PgrIIWswmaDEyAY6pgHLOjmcRKEzY5Z%2B6MqTB7MLtmzUpt%2FZdu3a9Z00ODUjFQ%2FZAEGfAVNzmA0%2FxiYqFwZll08uFKSGz0asu6fDhr%2FoujFMzKpOOvqSqqP9MnceWL6l7Voy88LaP4jH2UFfL%2F3RK%2FUrW7EU%2Bjuk%2BgMJXwTYk1Rh%2BfasCM63z4Bss3d2Aign3VZh57jWQN3HBvgxKzE4rC2h%2BWYEKSdtRTpTt18R4BmTZ8Uj&X-Amz-Signature=7a6aeb93c655bbd6a1ecbe50f06928e363be5469f0385b4cc97921f7bceecd3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

