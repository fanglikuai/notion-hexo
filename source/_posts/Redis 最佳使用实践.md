---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624QKD2IH%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T150059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDnMab2tQWQHw3vRObY5htJs8mUKLYMdCHhbzfAG6gxYAiAZvOBUl6p3EqR5NKzyVioV6M%2FBSjkBmpXqtIB2e0lpbSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMFxMd5xMxwm94e0rlKtwD8152f1syQvSvkbH4K%2F7NUakhmllFvtV%2B5g8kIHCmtTjMv0tkBiYbOu5TFWs%2FEZJ2lf4VQ%2FG0a7AIC6SebZCXJqHF%2F%2BjPXMbSdQm4EBbuTu%2BwtYa%2BoTzNkXvCE%2F9SQAKrJfMFoMvSUKIKKtM7EtJrumLpncw7LSwQgpszwSQQ0B4wQ8YlMhpJ51z70UExecshpKOpmtw23lPHRqZDuczrLjVKdnbwE4SB4R0v%2FUtgMGPuXK1KeuYXsYqmdRuCR%2FRugD4AX8BjR9OomCpeam2KGUuHDiK2r8PHI3aUrf5RpikR%2BwAF5j1YiLM%2BQaAEtipAaphHg924qifCQd%2BQs6Xfxn5qdg0CSiKFg5UTm6xAD2s%2B%2BhS9dIUNs6M0ZtYuGgMG9fYj6aBTuSc5ZhjO5wvJY%2FatITpLYXs2%2BTUA3Y7JJNjYxy2GVETd8vxwkhNkNCrTE92sK7HPGghoanttIisdfxdUuf5yPEwTlbd88pR2RagZszY9%2BYINw1ZcqUraEOLp8kdO5OhqUGG%2BPhwf5%2BR5zDgNfUG7C7u4Ofx3rHpEOCi0%2BpcYHC8yBpb%2BaHlAPCi%2BpCESsL2I30iZhkv77RPPn71AmYrcRdMwOvmyMH2mImC6rZQpuq%2FtHA6WqeYwopO0xwY6pgFmXxk6dd22A98ysCXSawNsH49nGww7F1y7SZuOwAWVZkdFDCqDLCNc7S3BMi6p1x5odX7T32xEV01TgRH5VQRMEgdHSLQ7UlHnmSwbJ5OjYIaaT9kOLlDOufEmSht%2BUj6ETLElEcVOTlxTL8Sfr%2Bq9ACe%2Fjyk5bhHUN4j8neOpwxvN4BSxkiCPp2IcdIiSy0dXEiuF4ohVY%2F1MUGVq1Pyjof9J6vLN&X-Amz-Signature=c7a77c6607f6f0e5db5abd435117fa6aba50ce12d89d135e1357ded52df30720&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

