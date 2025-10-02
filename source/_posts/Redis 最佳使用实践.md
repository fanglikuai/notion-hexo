---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWUANZ4W%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFUnJW8Zzy5AMDYTCNLefc016%2B1zWctGzyHFWacsj0%2FAIhANq9V7YOZpIkvHEkX5%2BSsPcMA0URbpgLBd%2FYOLJvYKoYKv8DCC0QABoMNjM3NDIzMTgzODA1IgyesaIB6cFJdGOpQYwq3AOoj5dIeuEwzGb0FGPHcKsz%2BdGMTbJXYO7G0kYHL%2B%2Bqbbqlw%2BgYlxKL1buqrz3tSBAha1a3RVfOrldPmvN3wVUmPJMrSttChj7Tz546X5js2IUOpbYJNVaGhNiA88X2CvyTpsnn9S1tjlBtMlUgk6k0r9s24oZ6%2BxOYwfOe%2BahFOwTRSLIZUGQerjalVjxr3z8eLMryYaKKEHhkfxADKyDS4tNBhwl0Bp4GpXlNs6I5ogH8AtQwvnMztP3GO3m2uWYH9HTUxSWxRKzfVNqwm691ezVE4e%2FhN4ZFqELvZ2A2I25ox5HsPZg24c8b25T%2Bloz%2BSFgdPvEYeMQEVtQ57tKa2oa6wcrKlwCW%2BG1i68CacIUv9dNpLfo%2B4H%2Fa5pvJGiHOWuL7WOzIXOZOiBN3itUNNzM66SRt9ijxjXaJBFqt8Lw6dr5jKTOSpHrQUTW1YcitJhbmsV8YCiYy%2BluSTkTq1PTh1B2CN9CsdTD0iXdkKy%2B511CPMB8%2B5s5Ilevg33jnTDW3pV%2BDVuwDjkik54ANFdGcxhLe2pk5Z9iIdpirLzwNweQJdKAMCaFCroB1D8i2YxiTNshCljb6bgflP00oPi0cg18GxJ%2B1chEJzZGbQdK04kw9J7QL4WRiMjD1w%2FnGBjqkAVryLpi8mcV7%2FpG0swXeQ0935WyJGZ21cSOazmJruhTxu4oF3WNWBDOvcila6xx4RYnzp2VrqehfdvgpuHc92EQ6aLN7JRg4IA1r5G4MZ0vsXU46b2X1IGCddwaAlSTrUbdsAjgtRExDrQ%2FCaQdlv1NVXY3qbPK8H4eqLQ1hqRH8PQA3kL8qLfPBayAW0vUG0bEglz2szRo%2B3Hlx%2FVh38ZB35yxK&X-Amz-Signature=8ddb62375afe51de4be1796ccb3abe4aa8a54396fc556afcb0ea3244a8d1720e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

