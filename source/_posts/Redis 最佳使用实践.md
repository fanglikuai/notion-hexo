---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZP5RGLCK%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGZ1bwyAFfgAYm1mFNXhtHGC7ZPIr3XOZFip870f7ZYDAiEAnaTJyt%2B9q60Lj458ZqiGKsgvuz%2B6ZvTe%2FZUFi1jOE2Mq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDK8E6Wt0mSJc2F0V0SrcAzqca5gyOCVccC2k8ga3heMse5Zue6Z1xKFnZ5As6xvTiO9xoYHKVQN0JWkTCxepZ%2FLvLeld%2Ff9w%2BcuCj%2FSlIKsZZOP3nA78bmjsZ7EO4BFUHZmgnYgsdiGTdohNksP6mmZMVQtEdwYX2tjtNOGF%2BjCpdHeAyleoTgp9t13iEoFll0OQNTW9vCN2JK2nVSgiB3P4nw1mz6I1OkEnXD3SEsKwX7yJ%2B8gosiUvDaoLnre8TlHKePMq9wUafEVbdUbf0zO7Bp8YFeDGPx5ne5Ch1WVhTsIE5qpFYd6nfG1D21w%2BEv%2Fm8tLf67fAYl3nwtHwpHH69ATc7%2B7CJ407tkYnMap1ztiMIUvLY%2BTbg1l21CUHG4XA4wG4Amf0RA9TJgWg8Yqk3H5cSMLGUfJsqglpjcprNVnNX5fv6muauMgeoLD7oPDVsRTwYRgGJYXPCrJWYx2nw%2BBJ2BnTUlFRbC5zNsd788lme0E2Nvr72HAJp5GWgXVIFKA3l4%2BZ0ufjODgDaYW0xTfAYBDegxLxu8x1%2BjDhUe7kOTpiokKMgdl2VHFPBCrdGkrpqnwVXv%2Bd0heXD1na1SmQdUk1Xf0xABT0utL%2B157pv7DpNazx79Ei5zSsZP%2BABoJgKNK9jZ6cMKz488cGOqUB4McnxWis3NegkNNn2K2Px9hIK7qmOsTTwkPhJ89jA9cKDCWtnd%2FFuIKSXUrwlFLEra%2B3LYMC6Z6nVBiMgjkZ9jdH6U5RxhQ2tC3miLGT8kdVCicFfp9w4hXGzNk0xpdqK6x3HLX06j9nD8phIDOy3nDDxeEOUaO%2Fhnr6z2MFfrlpyF0Ll%2FGTm05Fza%2B0dl10Kz7JXOcwMLe7Lv%2Fup2d%2Byfvrp4wk&X-Amz-Signature=122fe37c08918199f91742b79b6b67d8fb030bc1fad6d3d85f41bfb5ddabd4a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

