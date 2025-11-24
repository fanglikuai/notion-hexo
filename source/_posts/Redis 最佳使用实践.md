---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6F7ROUX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC1liHvDd8HCs9KqQvqSmFc0Ena2y2NENwQgKtN1bgapQIgeYYtNhWXfAMn%2B3NGoPJElwqWdBF2pG2QnKnIAhk44W4q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDC7dy7348baSzsMkNSrcA7C9u7CkQ02h4zEzAbOG4AS4fVXJMyvSrtZUck4%2BMmjUsKVWPPStPfmV2SOl%2B4CMI85NyROlrAcB1dCuXI6Bo7TU24BIlkVNrzR0FUjNRzYwzCt61xOTgJlBtGPhrpszh7AjRFExF%2BJ32d7q%2Bb6OsOtkiLztqA4S4biLNlndJwQGdMNe%2FqxNoynyW%2FugWx36m%2F7eIV%2BfzexLKGeC537jqX6wv4CvDdktgpsP68xnOFFInm7C5Hxn4o%2B%2B92gBhQbjq1uF1EKbBHg1A3yzqsSaLk1cfgNQbyeb2QvLEsSQ%2Ba%2F%2FlMKRplg5k%2FamGuATJol%2BR3v4pMnkhmhQS%2FDQ0s0ZHJowl%2BySlPsBY5teRApD9F3x%2BKWyVFKoPFMI409VdLgEAE8DQFDJlrVd7ly7Ev7xpoen%2BUSC8%2FEwL%2FBDNzcDBBiWxb7yFgTTpHXUVqELOSwO2D8bwEar%2FAxzjlwDC8F7Balf%2BRjNEZueEmT8ucjE0cvoOP7%2BGkUEp3uD1yENDVbsIsar4reOcbFerga5fSKLS4SBo5lGq%2B9SyCIgQ6tfrFu9hM6u6T58UdW7HNgjp0BUksnF7NDAg8LUMM9hFjtUAghK52tTM2T2pS3tNjHAA4aXsLI1rE%2Fu7W%2FZ6NJHMP%2FXkskGOqUBV%2Ft7olwhvjU%2Fg%2BG8tL0ceKzHsbiBCIQqDzSZgAFKwCCNDtZQnhOtkLsxqyeYQ%2FaGqDk0UkXvdf5bFGzQ88ioA5s6r82OGTg378TLxj6A%2FLOuM1XOYnASpuQYdd9O1UMnGLA0GbgQinSPfAivR5F4dfSDzj8G2n57Uv5ItJXDx2mdgx%2Fi4pkc1EUCFVrvHyXpSYZuMDPC0lgEXLAtf7pXI0el5FW2&X-Amz-Signature=f2affc45938f566a74e66baa306d3582b7bf95f0f1c9778277ef03882dcba785&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

