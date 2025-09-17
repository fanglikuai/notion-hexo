---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNCZADRK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIGHApIZqHYhQRUd5VnaVBtY%2F98Hcc3MS4bAPPgg9f%2Be6AiAo%2FyhkTtKGbiyqwsyqUbSDq%2FUxIz4LhQTdsgyg9T5gJCqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXGJ5xFPBiv6Jegr2KtwD%2F6JSuX1jtKbvFyj79F0lYcOy9kx5XdSm3%2FzeUm1QxXNFwFMQHLCargYcSYkkFNSYcE7s2NKOa2dTQrk9i8GbK12ceIkS8R5UE8tkH3Aivq%2FXAiicCA2UUG127qKPbKgpKiIG%2BgAK92hhSjG9eapOf41rPmOYwMSgoQMy0C90gMiLjARFccgY28YXk%2BV4PEyUnuFzht03Zs%2B1J1oJBmwjmRxtyA3Rp4xttGGNEyFXayOP8B2ptDfPpZnmtsgoFwqQ1ajaP7VKO46PBa46qtifM%2Bi16ymiVIWioGFKU3ShMpGN2nda5UG%2BmjmjKPmAzxHHoAQ3SCzTYhTkJQyNhNxOSh7WU2IC2aWgIVNZNMGnC3oXJxeFAJG1Dc3G3rmR5J0%2BpduClGcKjtvssKxmnYXxUCsiQYoSPH2L0dHutyvQ7eqKrXfwgKQMxCNUG%2BmBijEkTo52FwxsjFHi5irVmQbgm%2BJAQKb4S%2BUFZGOlwVi8hDfEE37ong5cL1XxcDqzsx%2Bunw6esA3CQBzqWoA9tM9UajAALTIw%2FsFkkSb4X2fCW%2FVTSNfNQvSctgT5ZKDPxLxVvFZuRzEvb2Tk0xhf7VVW5RkvzcFMLWEBgZBEi16E%2BdxDFih4G%2FYcWzJBO04wnqyrxgY6pgHl5h19K829lH4NmrqmuPE40HumPteLdlJJQP69UOlBWoDh63pYsK8o%2BagyUAm2Ri7YJu1m1GlUcOnCXU2X%2B0yOjs7RHMyhSv0EPeZzFsNWBRFzu33eA2JJjP9p%2B4JE3zT0XBGL9uE%2FOYMYHd5957EbdHQ2Y8mb3ICxXTFB5ocksPr8DvAQMw5ACSL99NCbcCdEp6ISOI%2FvDsfKJSHLprQ9bwDC5OJx&X-Amz-Signature=4038553f234c7742f902b5e9e7f729681d4532166e0d7842d6f185e11304371d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

