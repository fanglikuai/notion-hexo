---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMENVNYQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDjP1K6Pk7rVnbynBJE5dpEgfHiKxkXA4DswTtE2kZ2iwIgNyYF3lejb8hCGyy%2BNBG69qwRUvftM3q0V5bp4HFaA84q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDDHDfZgGrhmy5I0I%2BCrcAwKGFuwjJxSkg2HHwEC4D9gGq8a%2Fo5b%2FvwWrlI5ta%2BOqnkL%2F238aSmbvq%2FgTG%2B5NwGOizG0w%2FxQ0Vhh1E%2BF7%2BqGuHxtBUE8aP2UbmzslnnjnILBdw5m%2FD8gL0OWy2GEJWmZDPNeSj94hQUbOUCvttAPEDkCHqxFdEe9whXaRvaH%2FIksYWCiI1LFZoHgrqJRsAZ4BhSg%2B%2Fik7hNGd3ozaHbpFvvz7IJzgNbUpoLo0LOFKHjbrh05YXm4xRhdt0AKVH%2BNy4VE0wTZwk6gyBH3vx4Gw30FNNV0Aj8N2wYzMa3xEydqoR7%2FW5Pa4EhLywiE1Rk6iYJCv10ZfuXPOpJhWIkFXB37jF9RITIBHX7f7rBcUB8V5A6DtVDYK9ehOKecaQuLsF1x6TuiDQG0ZfIGmfOeMjHYBpBnTaAGPOd80Ov%2B4gvsP0AjQcMaFavxtSVRCO011m5%2BiGKtdI72KKDMtBP7ii8VxkQlIZPRXZEfFv%2B9lZLKkkKQHg7GRFsqom0fIEaDng4vc%2BrMyvRr1eQax47tWA8ld8FAEq4XHSIr7d9QnAxapNuhfPD%2F4pkAF7%2BxPNIF5GBZys1SEnrNqW1tZTavED67e43agtymb4pbIBxijfMT%2BOR0dItWOJMc2MOemq8cGOqUBZ%2B0O1Glw75lCgCp%2B1dt1fRs5LzipMyUAG0kDvKXdHhCH6GAF3%2FucDmZj%2B0nPITfWebIwWTY7pbCC9BJz7HDq1I2Yjdb8l4GCInuhql%2B1VVYz8ldTh9vRD4lDv5wMvcD4oRMvQjAd%2B%2FSvomeTCTOJPeHkz%2FLMWy09t1YTQbmFmkGN%2FtOR5dlPQydUcMEDKNJHG%2BvrVC9U30B3ADdWLF6kWfPgvbeZ&X-Amz-Signature=89ec2839ae3059b0875e350eda543fc59837a807467d59ae58ac2cbb1b37602e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

