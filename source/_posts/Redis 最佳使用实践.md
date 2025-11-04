---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PASSHC2%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGaL12%2FQ0Eepd0nNK8FsxAiBJTRWPZ6JEfxHmRqP1aT6AiEA6XVUlbHym0vwXxDBXisSuJFYHnp2WS1SNopw1WNuywAq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDKCsujg035IWROuSiSrcAwr12%2FaeVx%2FaR%2BEp1xuSdctW3vej7pwX72O3bXkaPqzhvHAXpnB3e05berOftSzqP9AiLbigB67x8DV%2BVg198XBtWIks%2BwkKhR2QpxRJuf0ZV7MawdP5XM9P9qdkHNOmEc26L%2FAtfYkQOW0zPFSwmMSRL57jGVDmLn1Im4wbSsP5dtH8eZCZqObhvLGE0u8uLIdWT%2F3e7suDjquT7GiWVGJctO1xWKy5UDBBLr%2F%2Fy6PmK7ufZsr31izJYkjtri2CkVw0BEoBn3fUBMw5y5lfI75pQ1T15NpkHrF0D7LMBJRSKeJm2EnyVFfrAwUshqRGSDeFgb%2B0mOsXmmf0pBxtbIc9EX0qr3PgdIMTaNkyNyxy6IccigNYaKCaHEMZnRv68tGmKs6LJsPujOipu5xKeVkeLL9sGdzI8uSYKz59Uq%2BH5uILkKcLJ6mcUT%2F8UTsdm2Cihb0r8a%2F%2BGLx6qtvLU%2FtSadXvKuV8ZC1BJKflFFKM1Wakvd2vFUWAsW0fFNzFCN4hy6b5qwgR7Gtf%2BiMTZ8%2FkaoZBlZIZn%2BrkZaOthnBkZvhnKTOj6qX43s%2FtfUL2XYuLEL1QIjpE1XItywqkIkMGzF6mmI9hsAhgqOv84n0KMa85dbs1zn%2F3HherMMTCpcgGOqUB8KtVEr9j%2BivoXzhlfDrPJumTbSRSxpMPtpQRrRfrszxDVGDucr0kDKPqY%2FLoLBTR49pKR7Ilm68MR5xQ%2BueJqk7czJQC4XahS8BfRn9i7DNcTN36eUaBBfJeCVZo810ay0JG4u%2Bg7pGwT21Uy35nomtqb4Mp8%2B8cfWWMXyfEAk%2FdLbsfSbAE%2BvvNCDCxQRk2UA99wfUnF%2BF8uTsbsf4m%2Fd6iTxzr&X-Amz-Signature=40e588e3440be1ce04ffec99b59c7d74794002a4ff118e2474d6fc2fb8011bee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

