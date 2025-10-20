---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LPFOUEP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIHMHvp6WrLV%2BbYv2bNc19yCboIEXP6yG5IdizsEjenFyAiEA5mYsEAxqHnoFFE9bsBy2upTMvQx3npHl2PKnYyhjrxoqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGP%2F6sN%2BVdDT%2FhASXyrcA2%2FuD1T2xVY%2B4PO7GM5B6HvQfoERlX2WwZoVBUIEoqEm0SwbxHIgDtsuaLHnT7x0gxQr9iSeC1A2IuRJPV3l3u0K7b6zXwrhXkcdGl%2BhG5lMZXFb6voNHL6tePlRZ7hc2hOdxUvl246bvmLsFMfMbaP167oSks3QomKl1mFkCvSMQYQGaoDnu0pTsndzcv%2BfvtVVnEdWngr%2BTamnHPfOfe9vQog6xNUSN7zu1Q4kbtbBuOa%2Bxzs%2Fp2R6Snb5ZMpxtCo7%2FXI%2FtUdTVCNaiaRucBJkFFl%2FHWrvWX05SkPAL7nRQlMxL23smK3EVIuD7TSDByPgDMUGxlVgEyoZC9Tp7vnXc3xtg4EmD0hhWenU9S299qxJ4rrVXTL4uVR8PRqKoAK4urJSrlqEcBVni31pwkaQUiCEgfa1G2T7Kgs5p5cEYDjc%2BCalB7%2F%2FY9sUQUVqi3mjeBh42fHZI3JLD2jzUId5LdJ68%2FrntKl5gMjo%2FsDP9mcAJXnwvLiIS3J9W%2BE4w026cK5l83tpyP7eFJimyCIOQC8j6cLOERkGkz75XZ3GSKmPlDDPG7om%2Bsg%2BBGq3zSrEzHxmnU5AO4MAEnqGu%2BUeiMcvxsWTN2jprggX0lMldp41QwSmNp3SfG8aMLi318cGOqUB1RDS%2FygmhHM%2FZtVx1aGjvirrG5aTo3sXnZ1Ps0hjZB7uxrHh44yOnARRJYtUDO0kIdOYB3MBlFXjfFXg8r2fDtt0wiuq8BQYUf%2BPXrVt8A5zKRsM%2F4fgM8q4L5VZIxWHDI2wA9JbdFgnfrUNzPPEPN5uJqBAu8A1nC1%2Buv%2Bb5dsO1ABSh4%2BREgXfSTyU99QTbrKd78XhckejMyfijNaiqgOinm7A&X-Amz-Signature=81f7c3b816703c6fdc7f03c5a1a439c2f499860e147a8cf363384ce712e67179&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

