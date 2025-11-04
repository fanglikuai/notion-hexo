---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTIVALW6%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBjZ4%2Fp5Agvt680hABeAnyv9nqn4C1ge5taYkOfMSlv9AiEA3VgtaZ3%2BqqgT%2BPkNV3QI3BXO1Dp8CH8TlrSN7PeRZyYq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGCLH%2B6mZe%2FSP4RKISrcAy7CTnTAj8ZLuld793aOnE6IWE4b5qghN6U6ZxJHNZkasBsZabXua%2BEkZJcL4pWUqpG8uDmLAObejrBunsE2Gblt1VQAKOKoFJUCaLvITzVxR3WLHXvg%2Fq0UI%2FEeRiFTb9cOZHHexaZjI3Nf8%2BboKEvwhzNCDuwgzklD2fNCDWWxXZJ1ewIZESPUtzPW2CaxWKjyNFgMqKmmBvxWIwJyXNIaNN0U307VNROOPik3V%2FSSrVDmtQi%2BwF6OETFGlFwyXynmUeXGLPXtSbSyzcQ93babdHoY%2BjrGLoBuwN5%2BrQ%2Bf3wiXzZ87wSQZdVJsyF1vxvfySvV41Wub0D9lKzDAuRfTFcdmtx%2BCTOv4DAbQ%2B%2Fn%2B69PP2tr964agM9Ny%2BS0NKLV2MP4BDhvGeTWkzTeLMsqllneNhIlZFUClvlwJadK6Kgh7IS592WXQ33sbSTi4P0bLimh8YyJyLJ0d1jCPdBTsgKJcnilHMsJS%2FufI7mlBdd7NzHmOl7CvnH2C0uvBJ2YvKkTUnDIjcxp5JluVCUlRVLWdUKd9FVmfjBLIRurqnCs2M3wvV86EPrXJqvinqHT2c33udYO%2BVkMwG8dAXFgqfj8RcSZh033fJgNMJ0qeuV%2FuNzHkZc%2FG32TqMPLmqcgGOqUB5TfPRjMvw2qm9ejiW0Afww8Nnim1HizuFTf2ja2wezRvUm08U1lAL1oqkEVqMWpPh5lmVObkNpYiOkzA6N0VzfrocXcCalfEFelbjQ6EAj%2BLoPROlCAtieBTY9eg98W8FEh65Ct5ln%2Fe1syDec9dJgtzuZMouJ1H%2FZyvXUs7XpdBJIHt%2FNNOFaaQkJRj5zGco0%2BrucM4kYjF7c7DAjqYGPJHFZS%2F&X-Amz-Signature=83f536f3074a420696cf324dbc2d70ca9dee58c4069e797fb6d71ddfd3903059&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

