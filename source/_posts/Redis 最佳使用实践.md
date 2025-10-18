---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NHHQOTI%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDk1590Uw8ivID3U4wWQXXl0Gz4qIhYnaOA4Je5MFWH5QIgF85R5YhE5tlhapXbY%2FJKVDIUMLRUWoHvngoE%2FmRO28cqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDIxykdaP7e2VQgwACrcA4caMN5V%2BLoGRhJVxcEN2F78CG1AV%2FlYkO%2FVS4rtOEzebYwJP8zBeHVBY0t5vpuphg2qq4O97XdMzShN99dBt7da6rmC0XrqBrBw3haaoRLpiXzXEYhLNWomCm22RZ6B%2B75uvW98iQUHEseIwwjvC4jw7yQqJH5nCOQmcpTdC4jdslwuo2lIfmfaz0fphU2TQdj2PHFA2gt5V0S8AIiFDTjP46Ka5dRtgyhxIdXL3mF6W5v%2FtC0XAaR28PCaLsAAsrUvJsYIilzhEnntu3uASvjtNSYhBXTECH6V58WOR9RtsB98cnptZYHFqw3tTj15l5A95yyL25ELy5gx2z%2FoSvb7647rulWRUeWtDCxnsMt1Q4M5xigYZuuLYMHVKiRrb%2BZZXtxm19gyJwKNhQ5R8%2Fs3%2F0Vs9D9IJtDW1%2Bm0dre6BBHRqlHcVpeZxdOyv03UZ%2BKPG59i%2FPl0kBrPXRarpdeaQVUtmBRjONIgBqWXw4%2BEqew3yHlDajlU4wS1tGxErfb0B%2F%2FBUlOC5TgjfIOLepXJpt7lM15kB76ZoKmdjJkJFlUx5PVi6Avky5HzHWmmLTHxvqkSIHjy3q36BGX0HmqHd%2F12hykzzs9TBhj9XwILzjTGw1ydBCDHqhtsMLvDzMcGOqUBjVcMtlaFlYZcyzMdk%2FHE8Hl7tpe5EIIDzeq0tPzEX2qvbRyU5lpk0hyliY%2B75vVeGno9K64fEeanibkqNjo4pmRHE8OO%2FER5E12T8VNro7icpnljTGmC9y2fvdsmrhvpD8M8O%2B%2BtAK03ToDLKVpfLL4lAh%2BjO0TAYFOAjuZTQq%2FPd%2Bu3FL8H0J6%2BU2eknVKkmk82TfO9JWdvrfIjRxbSfWOguYBq&X-Amz-Signature=7992a59161251299157b9faf5d53187eee00aa989e52f6f8498db81399559cb1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

