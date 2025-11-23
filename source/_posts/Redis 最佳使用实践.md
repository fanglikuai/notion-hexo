---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDUNMV3G%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIFevjg%2F7M1i3Q5niqZS6NZSaAkRHs3qYJvtjPWSGXJRLAiEAklhFipRIFIrMF6uqGEoNjMSkub6mLaPk%2FWx3kcinWv0q%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDpzPmDdXWGCRGEeXCrcAyRIejwlwUhQoANlEs21AV1Whqlw%2FgE%2B7SW8n1r9LysE6uK%2FJ4KZg1WFC%2BV%2F27MOo8yPO45VLaotoa%2FiHql37edL6xJzVxR5Ok0xGOH2JJk2Dxwn3b6EMZx2lA271kscaghQs%2B4TcpTorlLhudUwSHHFAAqxmKKB823YHJ7RuLn2jPWkkybuZF4sRq74uzlKrMqP6GNTJXTLD9vfKA%2FZK4UgC%2FXc0nBU2ZTGU9preiDNekMsuaWOYY2VgPlRwHd841qMl3MQ9FymXPhNiQLdtBOyl5RNuO9PloxHQsnCWJpxdO%2FKh3x%2BvHgFYIXwdQB4bO0KJ6VI%2FMa9IRr%2Flwlh5afBgqyd6uo%2FSeOr5BcDMRXeGgqIII5lBWcZdZrh%2BkTEU0bEv%2FgYlJH1k%2BkMr1hjhXvnsAKsl47%2F5ybAlJflWt5AYiE2lVCdGxwOBjSNs8ZTuijM0QAKKi47jmQ3Xw9BxETaDv2BKTHzoq11yE7Nc%2B7FwWAtxyynP%2FBBc%2Fp4DCqirHygASTDLc2HiIosmZUsajfAr3zvTViDZHn7EA4b8ZuIggqslCmS9LvB2mpdHc%2BJj7AP1p76SYTEzFe8RK%2FKcQB0aMnETGDyd%2F6sxLaUhd3Azh48ja7smyvgr%2BNgMLSPiskGOqUBGydC9RPiYlGNn4hUUaNw7u7ieq0HpGhEX18GX0u7rLinA2PWCb3gF9vKBUuxVKKpdTVYjsRrUpY9B%2B2QaOWlemPSY6g6DwiAUc25pp7OrvqS0R3G7TJqjzWsj3aL%2BupIFO3D0bTTyvqpa0ajwNAyUIXkp0RkJWAuvEI%2FfyPw8kAVFKfX0aa%2F8HFl49ViqWB1ylJBAbAK%2Fp74mJLJaisVsjcmcWSf&X-Amz-Signature=39cde9db6b94200d41a91b13fa35d91e6b2151ed46b4afa5751f4645369873f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

