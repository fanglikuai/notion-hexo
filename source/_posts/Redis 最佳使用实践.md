---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSNHKA56%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T130044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCICsBng5aKj%2Besx1IigHMawp0qar2DI9wATdcOIfnephwAiEA7f24VMKiPEimYH0L9KRF3OapmSjugSF9YI%2FaDqtzbkoq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDLjBoXYoA66HJ%2Fi8CSrcA9Xw0fVwmDL2fKn1kXYHWlyE7JErWZBvozv7AlUV0C0mrhUywjWSQIQTjhTmXlRV0J%2FuxJPF21JD4ZgCb9I8x8OrWDLJTW2mLkUc%2BzXlHOD9Dl8u98FFS6wyRbe057SiGviDQ6mrwabk%2F2eunUEqF%2FsXS%2BolbLH2a4TGVWaIvaYMKaI8LBNuqtPKoPP50D%2F%2F0r5sPwPA%2BvtQx5DfWwQsM4vYjpPINvY0Jw5DQDB9ZUqiatSnyM7SbmPR3gTMjNsHAvco6q9a7oYXwaM2R0TX83Jl7byzRhdqa5K80Ruh2r5Gua6mCnKc7OhQ%2B6tBpqReclK3a2cjPAGD6YQmQUnsTgNcX7yP1Nkx9Z2sBVKa1FHwsR26aL7bq2m20v5RbU4dssI80jskvOijQJ%2FynLsJ2ZYB5w17Bq4%2BGkNyr6AEoBoC6%2B%2B4CyWPQJ4p%2BSXzjJThDroRCa3mBPVet3vUqIDKAsGVLcVG8OA2UlLCMLJjqoidExBKCDStFMTcsDd%2BfX92N6Ry8bGWK19h%2BOqDpRK35zVGRiXoY33weQr%2BbZF1fdJVbPuXgrAoP1EzXj%2BDsS7yrRnuVnS0iHS84%2FI7Xf0QpSTgfcPaxMbiCbotydQwGzRrsKgxAA8gKSVt8Z%2BeMIDbksgGOqUBUaitUYUhbtzOs%2Bdeh5QTa7yInMZLX8it3Mnj5zXRHYf%2BBPajGVe00YT02bPXPGFQ%2F%2B0TYu2QyFiNvmuLbtrGeqFWRfbINRgPmp3fUIHZVqZQIWfmM1E%2BMjEycTGrls7XC9ztkWCkCZ1VJmR6sHGbDByrrhRVw4rEVlu00noQDi5IAcnahMEfHmPk8nyJBeg%2B92DzTQyyUOpurWXgE%2FKP0rBSg524&X-Amz-Signature=4266b1de71bce98e5c66c2aeabf5824f769187713ed34b656b919e46d98e1986&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

