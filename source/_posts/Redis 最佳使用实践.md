---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6LPP77%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T110056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFhsyikuge3MHGgVyumxwTYaxedkjIZlCPsarIKwJ75pAiBgmDr9D8Hr0F%2FARnAcPkGpRiJq763q5i4G3Gv%2BwJbFiyr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMl49xGuKI5iVoLuY5KtwDWL2D5vGGxaQCCKHv5%2BUB9lxYgECgHAqvsBISO%2FP0FjHI7354z4XjGDfuUmZut%2BNyhR%2F4SX%2BmRUOj2M9wzEkPvhxdu1QCrZeVpDEGy61po1cuRsFSezu3%2B4JB1anZ4xPqjtoPXtAsTAnyrtmPhZcKALJ2lB2fvxhAEMl%2Fny7Z1ysfRZMMchUPdkR1lXCgEvPauHDo8whM98FurJPeKEHgmlJZvz4cUBKlwtL%2B91o5gd19NCX%2FQNLKcDcp%2B526VAYFpIjJ8DG9vhXcDKJ1as9a0Fs2hlVioVrIBpdtTCYuqc8Uv%2FD5r9Lc%2FnceUhjX3NtQlQ0YQNRO%2FlIYTvNb5ehmurioyCFfaw6riPvqJ6pbxkGuh0Xd3G9hsUuawDOxN2EHdlZDEd5joIOhuvYpZNfrLGwF%2Fed0R4uHCjADkX78FNP7xNy0OhNRlA%2FNc7%2FXl3SLSI1b0iMrfHDlC5chxTGYPHMu1wJdpVSFEONB7jqywVuVow2C49PjPKdEdmkaqugiPDgzDuF6jGsjZnuv7z4u%2Fotc22QKm6ji0jlXOc%2BwtmC4HMJkRYR9ezxeMCvrYKuULwIBhVbRFwBUlDzxO8zWkEr%2BbKA42Wg%2FIFp8x5aeckRK9uz4rugf42%2F2wtswqYKIxwY6pgGLhHxakt6kSiNQii4uXsimky0z%2BZj7AYbD2Cudh%2FGkvNW2%2BvzO4qdt0v%2BeH7X1wKLhf7MGORB909uM2cJeGYfO3IXz7OM6i4OJA9orvKNOOa2NR%2FW3qFsEh2xY0u8VIMXwtErS4ADdHC4vMnwHE%2BAlVs8T%2FPCgo2m1b355Dzog8Rb5h5ih0AkjbR%2Bb975KtIcxqvys56nrdsrW7OySUTZHRVWScsLX&X-Amz-Signature=1d25ee893e452d66a0a920527ff4a7c33fc71cbaa942bfb821021c3452b45d3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

