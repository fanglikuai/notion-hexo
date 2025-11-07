---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673BQDHJ5%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3npDN6epBgnIvEkDqQrapMxg%2F609B8kBIw1VLyRRqZAIgGNAGXUGGPoiFA81PnrlqWDdj1wugOpsX%2BeVuzy8aCRUqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIFwFU%2FRfzME%2Bdr6jCrcA23c6YtiBlJZd1rTMouvSzmWyc5wDPAL3jijWGhFFDusHDjnRe4AIp9AfWljPoOmnfOHbPyS5B5T8LRr3gFuFBx1HGolce%2Fsca2hy1VkCaLPlasYSTGEo8ZWz8RHafrsAcygFMFC%2B9sQGpbgYV9Lzm1GXMhvupNC7b4tMCrLF6%2FCvkc2m6zo%2FKBkb5YDXSS3tiwnD0fTsT8dn18DQw1%2FNwOImXPNWkoflvsX98GE2WC9qkrDeTDa77KGx2bPK%2FtHYLYI633ntKnLDHQmHUhW%2FH76sclaCr4I2XBthYUeXh0CGZzp77KZEnrKd7%2BLVg7qnyFK0l7x7PbFK7a3Mpm50lGsvmasdpqonEopMqsvWpY7qbemSkr9Uf6L9LjfrmZ9EF7fT5t0Ave52lZxtc4hZqvXL%2FKGIj8JYKQ4HMVX7hCuIiX6dU2RJSA%2FA2vSu3eMcWiTafYZP3aZPSF9UG05JpnIr9sDUPjdKB6VT3biJ0KImuEu8MlpSiAF811RA9BRPcREuVwDojyjxsD3ZV%2BOCXYEom%2B9q5gmmoR1kSoMg%2FK3Qr99STwjPYbJIvj5583UI03ikSSZaIr6nKoF54Y3lKP0%2BYAnDjoPExCXIaf1%2FAw%2FNotnTHGeT25pGW2lMKX9uMgGOqUB8%2F68T5qclWUJWc%2FKm4DyLUPv3ar8HddebZ85KpQg24KejR9WFKfs%2F9kYFtCUrSlzs2rbCZLv6aKZ8OYc%2BUNwYEElSc%2F1KP0z%2FWoL5gnneFFKlYvGelFzxa%2BD5EIuiAFdaN5QqNUmYo5oSRjgcpMZo02appm8DA0pcqlaK%2FelDc%2B1sLaHZMCaiwQBOHEaJfrLg%2FbTywaMW%2Fn9ZXi5O5ih8PpoMQCw&X-Amz-Signature=fe389cd523bbe0c97d59d8bee3d1281afeb4cbde9ca0e41d1ce5cb8618c16f80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

