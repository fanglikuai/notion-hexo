---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6WBGXAB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T030109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQCyj32h%2BacCdU2JLLnnqf40RTIh%2FPIuJ%2BkNd%2Br3cYn6UAIgT2KQbcegNmHbUtLTx1Ocb214tRcZuag51DDr3e%2B4tlcqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIr%2BfmYuyU1wZako1ircA2152UWJn%2FUmrIunjm6er2A%2Bq0L4kJb6EwKDMLeeDvVyy4l301qhnF0zzTh5AoA59pu9%2Fg52Jbi83YEVAoFODsjJ3Fo%2FhqgCsKufSeux46fRrPfshiJz1TI%2BlrvULDKbxtX4oCzb7tr9490Rwt3Ll%2FD%2FmLiyJjCdsL6rcsoFac28xLESYBb%2FxHYAM0eP1hlceQMpPqoI7VpW2R5Sb%2FwLP4BGGS5trFjValy3OiMh8hW%2BnpIFLfyFRkaT2QcnbC9Eck%2BcjnanEA1GPX%2F8GUwZfzrxf3SKG3CUMMOIrnmr4bDL4wok3EJFExcOELri3NfDANkfGNOAbAtD9Xfljs5jM2DptMJTSqzlLo8SAcwmtt1Hcl487VEFR8stZU2WJP9iIzbqqaaj3fGAWf8cDiIiUD%2BsJdBOO4RNLxo4H239Qsg4Q6lBRq99aCP5KFehsWSAmUdJduoBczUTx8pKsF%2FQgQenOsYiaUhICNyf3FYw8%2FQsr5t29e39Lk7rfNzJUFVqx8fxd8obRkV4Jn%2Ba2k%2F%2FUa50%2FOaqBj0qlVuJ6xbEQR%2B8EJWEZjrQyRmJMGtPBnUl6TW6QGCJFh8Aegv7MqSXjMznsqAjoJNiih1NR2%2FyNx8EOhIUmqKAd01t0qUdMIWgssYGOqUBtMBsxygJpwCbsiIVKh%2BUt6rRbS%2FF4Dl7nMO1WIj%2BGNca%2Filb2gaL0alFWveNUOa0ugAwxuHhV817QInNw6sMEVNvGrqnDkp1WL3gWMia1w%2FqhSdYd8QJeuLPlJysk5z4ROGYCCd7tbkcdJF%2BRnJnLI%2BnP73jxnqjS%2Fe4dN02CN8K%2BJQyhKh6blV8uIibo1enRVTfyhgkDTLUTVRrWGz0nEBc0StR&X-Amz-Signature=cb269598d991469ad6e2b6b63393bc45111e4f0cb95d738f5e723f451c871424&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

