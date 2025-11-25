---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYR7FWER%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T100104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcMrbjshJU%2BpjCn78HBNndRWaG0K4uidiCKwn8tKS%2FWAiEA8YGHNTJwD7%2FChel8g%2FmS8dPg%2Fp4jyqQcgBRmYeG8q5cq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDC2V5esG6cy%2Bvmz7tircAxGVxT4nkgbPTaqyAXpWykj16yJOJAUuuSWbsfEuT2HEgnxuYVRMhKFpd6U%2BK9N3j4AywyZIq2G7Uqxztl3dbFtU2GwDzPSzcNbyZbnCX%2BB%2FOEKGtopRsNbE9z9ap2%2Ff4al9FmBMQ8y%2BjagHzWe%2FF%2FXATsmfewhyGlC1KTih3iQeiWWxDHMO3oTiw8My2mvpYI7%2FIUHK73Lnkwn2kSk0jMAxmCQjfjlTB4UuEMGOgz5jgiSt061A0r8bAhtEiuxMHdMMqugSMtzGKYkhG5qQuws6qEioKjQLBkzUuhA8eJc25R%2FpTNVfIwDqxkKSQTSmva41u%2BcS7Rh9c%2BxJa2rnagNdkxrxgxNUZel4LB0sL2V6geFqoMwSvp79pBTBvNAEdVua8EMrHAGalGWJzqeOTqTJJtW84ABLPWnyjaJUDt9UJsxvzsTF%2FgUgUb4lygdYCsPobx3TNADtNcNdToOuaGump1DJCY24UyWz0wZCziqccLL9YQBFXlZxVlmyEnTeg4c3Qx2qb2wm7BpFkmTD813dg%2BW9BrSPVxOr15WcTc0RCRQXWi6y6r35a8jGhnYdTL0xA5eRhCqxGWuhB3kpIKe82rltaB1VoIBr60AmHXwleMqutSo2IIffgFiDMPrZlckGOqUBktChLWl%2BzaDEOrs%2FJJZeJpHpPbfx1aPdJt2o6EPipYQWfrmhIpHul4q%2BD3040Bff5Ml4DSCDeTa2wugnoMD%2F%2FSPQg0oBk6N8k8WvBOQ08TD%2Br%2F5aozFl5Wn8G8Covs1JlUr1uedHISRgWNu%2B3tAKPyLxTfTeDDpTj3%2BgT%2BWWa0lRqCZyLhc%2B%2FnVFOKHeHiNb6n7eV5pzPExdO9Dds%2Bs%2BZmyy8I7B&X-Amz-Signature=2ff15478cf3048ed6cca40bcb3e08ce7e718324bddc2d965738e41fdf57750db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

