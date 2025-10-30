---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677K5X3FP%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQCjpyeEOm2PenfHuzYyWn2EIAlIhVtY4r%2B%2F2JaBHUjAtwIhAN9lmLfSzDcijHrRt3M5DRQKFJWX%2B2pFYP26kXqBsOsXKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnkS5M2ajd%2F%2BxqqNoq3AN2OjknvQhuTXfoymWXdODXvSEjD0QzUqi%2BwHqUbWwkIKyyEWdkzyp5aZXctqAxbkrR44kYFx9IGNPB5hh5ZHosxevVdqtU4D8e3KAp%2F4IqO0eNxGxPES9Ry%2BIpW1jI%2By568YcIN28ardfJN9INkZogZaxM8MuzJacTYtFoISvMdYYPdQ0slglSCFeXYdSbmmJX4FNq%2B54ck1GYgmsReu%2BskGlOs%2F%2FtPcMzQqkER%2BRcpmSFOe7ZSbJidv8XcBTdl%2BWLkf%2FF81QOLpBMCDkySlvU%2FcNuIjt5dnC%2FbqIyLOZJjkL%2BAtgCCPQINU58LvUx%2BrwuKmQl8aA0Rxy7078od27KVpbSA0bOgDblp5LDAKxEtnHgiKVyQkrB%2FC46zty%2BwvTqtRZ2JAmWdZDulxFdtIBUw1Pf8fADucwZuc0P2aDp63Abp7dmPug%2BGjPEfCFGOq672GDfygbS%2BnB2aRxjs6bsRZKcUSbZGW%2FN7CJihrHcx8xDd%2B7w05so1NBI0SZjz%2BZVhQ85t5HMkDTCW7fn9olCUc4BIbb0Vh56XyfHSgjIEkEOS2oESqkUf7QT48Lm%2BxaJLvtfIkcskey3f1Wfwo6G8XEe1VcCVB16vvH%2FDSM1SAvm8KGM3PLKQkXIGjDi9ozIBjqkAW0iZhzj%2B7Ci0WyU1BL3nhGvptIaOQFnLMHCacx0fgn29Mr36yTUak8Psk%2FzF5%2Bilnhj%2FPs%2F4xQE%2BnVW3W%2BE2atgkCsV6mUMYifUTPLt0AjbI6hLBYio%2FkEE4lvo%2FtYdUmVKW541b5bTZsYQq1U%2BqhiQkbX%2FoqkkJo2vmjLrCJkoxTCK%2Fdfo1mY45ejCcJesK88ekSWDpgIKUDdw20HOVnGQNoJK&X-Amz-Signature=b1c5c78dc085ee62cf99d1a234052185ee0090cbda4579a2455338dadd3bf184&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

