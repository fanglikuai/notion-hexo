---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZY7OX6NL%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrnzEATG9VfxwANfsVdM82NA%2FkjzUbWpMwEkfDB50SPwIgGuIUYktxdFUF2V%2BaD1jRPO%2Fi3VM03INCB9TsoX64xNQq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDNbWlealqu3jSoyLLyrcA4WjlcFUKSbAEHYuFbRBifo3EabJjW0L8yl9hTqp3NYJ8I1TCPYBg1aCqTePNhD5JnaaMy0Q7Op79kXH64RDKJiVP%2BdKWsvjholGsrxBv0pGV4Wb%2F2S5OG2UtVsALaH%2BHGvcC4JEChbnUH7qfnxoJmhaNChAAV4g2akf21fpCZzqGrdm8KrJuyPw%2Bv3HYyNNxf9np0FkdOyRB%2B%2FI0DazHC0ZNzCAm1CSL%2Fx81rUwJlWuUPGlKeF8PbZ0gbXWRW5WR8LX9bgPyMAkjCiaZRgQLw0q%2FO%2BX0z26D28%2FRErIq0KHfIpICmjFtsLR0qgZ327oB6easn6P%2F65dGb6dpxWSIbVojo%2FncVXBIJXv%2F4W1tWXCgic2dCh6db37EHSDFzK5Acsz4btvapIw4fP3vU4kLHUg%2FasV%2FUdRwcxsD2mc5YUuA7Hru7%2B4JpCEFONvNdhl6d8%2FChjb0kMIoim3DoiKOCNrFU9h%2BJ4TrFdRTD8VZQ%2FTKKtP5RvsTMT4%2Bcg1JjieTp8xdI78w6VslikUREQaTFJod9kjtEqv4RLPh1MErSnlafROPG5gYooVu7tF5SyOc6e6%2BbjaJ750eNNFoxtCg5RlKfqcpu6Je5qpBlEWhSbxTabCeQxGYHEhHamAMPb%2FgccGOqUBhpuERkZO2ZbPi%2FMitA3R%2FVWkr7DGBuCiG9TOZB7M9jIa1hMOs0oi%2FPMLQDoiutSUR%2Flc2qBN2mC3rs7VPb3c1hAqD4vqtaavFyqpD0or7OYlNf2FgB3QggXhdPX04NsdtsZKsve%2BnUHTpjyOF%2FDLHaeLBhZxTRerrI9D%2BabLoLEhhBiXMm%2BW3SnYNS4MFBKgAdA1l7ZAO8h11VXZxSjsf96WGyUt&X-Amz-Signature=5a32f73d9e450c1792604d738a12d8659f37bf8b7b3eaf4d813a8be46c3c87d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

