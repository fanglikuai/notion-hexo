---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTW3GOK%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFGbMXy%2B5N52mxIjIEj%2BvENK79zhVGpm2e3ulzOONvTgAiBTCk1XmcfagjB96cJbL6py20SAFueBMXThN3MVEHl6Cir%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMWYAZMMBmpP3O%2FwpyKtwDGrb367D6h4V7rpYyaN4795Or7exJ%2BwdH8v54yfCMZ8%2F7gkpahj22f4lhzlkRPoRIAlRTqAgWMZTw9eLLD5dUJoXLF%2FWJM4UMgNhLSeVtpXaTYFVU2D2Anwdq4ImID8PKPFK1I4JznxpwfffRUp8LX6JEINTp%2B6kFiI75RgN1B62HGEGMjCgKYbt6vtGeUY5dYN2lPS%2FNEjz2eFJKgOc1p47%2FTtP8FaGA4N%2B%2BftEup56der0Ih%2FFEGSlhU89Q5IHwX7D%2FCa8cog4DliJXB77ulECliQwPSGVHfu4Vq28uirPIYOkw3bKAfnmjaLlOOCdatEgyu67bUhAgA6uBt9NDVnKAAPTxfsaxk%2BHMMJhqQaChXq0WNk9%2B582%2BMBHPfDs2iuWKQ9zFGRkKL1NZdAEPk%2BGG6Keu1IW9RCPO%2FcKShlRboMC3GTNvSWOh4RxUzHUfWv5uC9fPVdDOC80CEBlNP0rsaX1rYez0idCOJxKu2Re0bBHgWoAhHNMuyCxZET5qFn8DOuPtBqs34X0Nx15eChn%2B3ibWjwz5TuBTfmJa0myHyxOskz0yG%2BjbHe0ycuPXnmpGBtTO8ckb5JDxJMtRgvEbncFk4yYH30jht7zXyTpVroVstZxWbO9B4P4wr%2FrvxwY6pgFcjzedLpzloWV4C%2F2z24YeBRoHq1GoCgRzbLybh9Eu%2F%2Fy4brU%2BgUpdaIo%2BD8s5l%2Fd2bPq7FwBFx8K44NTLR9Nu%2FBCH90HSkMOeSd8fxxcp%2BIf3J5v9p68ZpHhh1qBjxfd6gSArVZ2FwoBtGCVnbTFS6eWthNELkS1uD%2BURfr2VzBs8ZVzK2A5JazGcHkkEdY4QSYPia0lZBD94rbwRMwQVrs6XP3YQ&X-Amz-Signature=cbc99b2009b02aa8892579bb2288a596ca7fefe69d6322d10c28cad2ae79c377&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

