---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIIC5NXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCDDcOGDLUiViNC1%2BLr1GMCUTxWbN2sTFxx9fI5Dat3WwIhAI8SHORk83VSXnK0WKpMTU%2FfFbiPpEgL0V%2FRytok0%2BGaKv8DCFQQABoMNjM3NDIzMTgzODA1IgwVddjomAn91FO8eXEq3AONk3S1Hf3rPyoVFKQddcvPdegm66UySniu7JwByPR5XNhUc2m6CsaAC%2FH7upz0yZspMjF1cQjvfma%2BM1I5NptHdL7fE5LPUzgj84Fo6BEqt1wPn%2FeR4B451463Sis43ESQhIKxYMIgSAhwYkwPa3CS0fZ7SWpdv%2Fm9dtDE%2BQEq82DqfqXw7vGd%2BiZJYMCZyB5G2mwe0%2FUgTn9qBaE3DwMGyEgeY0kYVHpAeNf4HOS5TNbzaY1LA67wYDJ7fpgRVmUXD5DRObJDdTSgrba4ksPNO3XfEgsHSte9mLPpznvvAivP%2BvZuCKQ7leiVhSZWa6XChly5MEimq%2BTmOTo7XV8pyrMy9gIUrYfORSyf12QcJJpgP64CBB5%2BWEeJFQKmcGi%2B5B0evThjeRQDV5kuE9BZ7l0KHoqj%2B7VryhqBObGUzg2qK1vm5HW%2FI8ALxZxEc1bMMYUxaH%2FiCd4ZXG14AjDwTZXE5xiA4euDIGXXZ83TQLYpSDeFSLFXQiORXazDDHZ8%2FVOFx2mVs0J1QCCb9Mx%2FM7pYr8qCp4OX9JrmGydHAGAxWZ01WnssMu9Ul27o%2BFUb1Yeb2qwlJS2ZRWGmn%2BUCuSKcCS%2BL2qzTSK1TM4azNsYe8WVr%2BbT%2FYRf0oTCT0NjIBjqkAVll3gtRuz676hxqNMZo1s1osL4MpCKEw0DXzKeH%2ByDnxMXVE4%2B2RXvgmjemdBWKfsYj2JdwC9uJ1do30F3kFAkphxu1D2PW7Rg9pabmFag2c8ceAd7wkDS1VVIXsk5eJibc%2FIvOUFOESnLFVuzQ3vFJMDamwKZDr%2FQzMwTzR97NmEZBnStPzTptHmMACsbVcrdC82kQ2C2%2BaR%2Fv9Lm9vshcnzvg&X-Amz-Signature=165a6e008b9a0a61e6d3fb9d81b2ca71384a7495c9a80b1ee2998d370b8c9c5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

