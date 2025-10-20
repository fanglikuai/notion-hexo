---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W32RHWJG%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDTATEOwraqGAmonyszGQsWK7OajEqZo3S43GPn65c6XQIhAPsGwkf8Pzlw0uWnTmzBav41cIf7YAu7fELsS6eGXZ59KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzoGpR6xWunrTi06rgq3AOYgp7WgXVwMa1qxZ%2BdrtVCxrGLiVswzTHFxi1XsPDT5FSp74pV0HSIw%2F6GJv4eh%2B9eYIJgqfCdBsIOLXo8Krd5SvRkhO9IaxTgot%2F%2FZUywhgycQiHSJoqHMxqgMG3bchbTe8FJBJX3QcIMPSVTfsEvBjeMj6rwPoO5bt3dWJL47laPveztY%2FnZAEyCA%2B6ck1El%2BZRd%2B8JBwhuwF4h3km4HYkaj3R%2BSA8uqi9NCk5EFBCv%2FigCHh2GTEAyYfDZab%2FbVVwyUZvYzp9VwRiTuYqucs%2Fu2k7w33mRtNBfxKMNKNd4Y%2B01MAUSll7CB9RzfHcrDnG3JIx8zjbaFFbEEiJONNA1KXyhpFsZJei2D4g8S%2BKEOvf7zRDoQYNvl0eo0VknLt2sGLHJu%2B9ccfZ81ZWH1LqkmIyCntXh92okV%2Bxe5YiVghbmKAJF1qwg2LAoHTd0mA3bH1Tg9x1fneB1uN0NdtYSERrdz%2F2b7sQGR%2B4iAHLWvJmiPnteJFP31lcjrEfHd5w8tSVqi02nj3pS0PnTCqwbnMy8Zk3J6Ox2t6jgw7JvoIkFD8VRYs8gXZnd4Ea60zc%2F8PTU5VsnDTWHGCojKbG8JUBj5Pk%2BOemb0apJoj03GrQNdcl49Y2UCejDN%2BNXHBjqkAX1lXLGGPr1rVfhgjuu6ACAPKFuaFsDL6YXrekGNT5a9Lzo1P4HvMSDz4fWlw3KK4EaQ6wjLl3astfVXlnqrZcD%2Ftmh%2B1I3KHR9k0tTohE8eLmY7nRcjl4XEKWBiG9iatoY44Ov0wSIHCbNYVpvkdb33f9E8waSW2nqIIoYj97kxRYLqbalMoYn2D355WIPVvcVNCOHMo7wFLmq2W3SzNntFOOFH&X-Amz-Signature=95a8a6f97f57c3eddfed5c495cd2bbbc1d7b5b1ab010a13c24e0a55ba5149996&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

