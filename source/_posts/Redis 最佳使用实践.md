---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2TTP6SW%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGzbrSc1OaMhzvBhoBa9Gyc7wvRphVJjmXASEXufoZ6aAiEAlnSWv4xcoLedGxnODAJfCRa27pbwMLijtl1SsqQErhgqiAQIvv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHNFDO56cRaaMpV92CrcA2Xw%2FTrU0mUhkE7du58KmuAs%2BqjWZY9zbtbXn%2B53eiwqj%2Fa0%2F2TuveAFNqN3vboIa0cgHLxSj5C2rWRkwTZVvNg36X5rrD3TU6%2BmTnRyjZWYNVUuII3iyViGYhbUUjmFABfae1SZ2D%2BpXTabs40dZI72daFVQmTEDMVrX791v4AxwX%2BzKl8E63EAAOh8hjp2gL71fsX4iX82bGXozw6A9MGdIfv6zpGaaiKeIZJBuqJit9JFEEDjhLBJ0skaaNAlvx2b%2FQzyPFwN7Oje2HH6SR8frb60zhMDDekGPbiOnB%2BMuGMPw25lAfLCiXIALV5Zx4Vdr1xDX9h%2B41IeelaQZqLjAe1C5sRhavzWHKPYaT6jw5UuDZS2v60qVM1D1L4t%2BiNAxskDe3ud6ZUwKydrqSP7YW6eRKpdVWquhH%2BGMzdqyGHOFbDE6fCwTBtRnIGzRlkpd8dkV3BHFRp3flhn86V0V8LtsRI4GZs2fNvyrtOtWPPuvJEn76dk4n7F8klnhser6Z8pPswtXeq1pFI4sERpxvTvqfAF%2Bw4HbHVHH9J0dBl%2BztE9W3zPZKVcWoxQ6KXM684v9gXiY2dgr1Sy8yX7eMeal2EUmelgGwmr13APZprGRpyVAszWBiQfMITRt8gGOqUBFZzq6HD66Fx%2BaQqYOTyazLTu1F%2Fokb0ACfueWrooPFWelUZ9FqQlnJAimXeA547VWADAIhjXXVfAkFXsx7aoEUVYLDkji%2FcbdNKyzw%2B%2F1CtJtYDzU7idXSOrtsKujj3fpA29rCzh7r3tfPIzRxK3WbC9vg0cPD2TuOTkPHVX4bBxX9jQ%2FAwELdWBSqnyEuYSKF4jU%2Bf8paMYB9AeTR%2BkBGn3s4Vb&X-Amz-Signature=11aea59f4c045b83f658c5c4afad6b5e17cf5e5f43002410772bfeb98b757c65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

