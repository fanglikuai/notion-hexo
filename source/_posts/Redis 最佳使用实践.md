---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKMPRNBH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQDgHmn3DFp2lIpGzxYKKKkU3M6xY1e1aawM%2FM347aOIGwIgT22jhMCYcaQgv7yJ17cifgkS0Rq3v4sPZl60kdu0LWMq%2FwMIFxAAGgw2Mzc0MjMxODM4MDUiDOwNc11D9bvs%2Ffpc4CrcA%2FlnYWloswKgZD6OFYMzDlNOIjPF%2FKai37AnDTSern1l6pgTgpvFNZrPs3yREqOmS3yuQrSjY4Gyt9PDqJ77jpyLkoSkmM4agOR7QzklmnweitE2i2haX9lEGE2wqQxxXzzAz0oTzrJ%2F0xn82HPStw2X%2BdzAXgqRXe2WV1PejEZB6H0vsc%2FJlht%2Bsqrmz1WQZ7VzvdQpR5oCjDV%2Fx4RPvfhNHYjyiExfbSGYhtAF4P5wi%2Bb0hrgEvRLz10LD9mJ2%2FY9YaUpJ6x4Bsr3arLkrr00NV0gPhRGd49qr2MOFJct3eNEOUsuPjz4rZRpBXM%2F70Yxha7qvKTAa6OdItLu0F2dPzyPNFpzmMOQkgA8Br45K9Ri9DdHUzPwjTLfaOfhhfN1DrsZnEWJHiemmMwB5NKfFF%2BVNG3J%2FyLmpcLGaSm2XXmApkC%2FkDWhTzXrpONqKe10GqDZOq9sr%2BmcVskKaEEvZZwlxqN0E5eYMNpO5bJuRM4bYA3uzIVgfkxHfPGW4f20m56dqftcA3k76rDOdHx2Hw%2BNn66HeoTAjVCbmzgRMVGqX%2BXxQtgNSm8F7F3xNFRL7%2BDioTK0NuuVWhTvjDTwWlKbmNR95znC1Zz%2BTjpLgwP8KmdaMeQyIAH6mMPD6ksgGOqUBU%2F2MuUvx%2B6%2F6HRpG6vbh6a0%2FDzS1d5WWpbX4mI5PUQdeC%2FeIRVzs4uWnrZY%2BdjHag1M55Ih3MFM9Y0K6CEEnDAHgjRmpPhxKtS133IpO%2BgMWVtB%2FwIoLt1mNOQkbuO%2B%2BFlwSoNffE2UPhD94X9mn8bmT2lqCIS%2FpdGUhaiNVkm%2BzuoFsz%2FlzdqO1JR3OvcDR0pxPlwv95WkSYhbKGp9Rh0InhFGi&X-Amz-Signature=36c15cc57e250cc73f2538a672716fc4f942b232d8274616b0499342448cf9b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

