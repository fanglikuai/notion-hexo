---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNTJERUB%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDdeo5%2FH8z2hkrneqwSwRxj6j0mBh%2FZ1Il30VF2%2BD%2B9cAiEAhyf8MFgIqHI3DpP1bXERutrsniT8mhxOvEcP2SVvhHkqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuSMbSaH052%2BsgnzCrcA0OBNzz8bcPvZbSNojVMsmPTp9hGIrW42PK6X1B0XZ1%2FjxdSTxsfYT330P%2FZvSEAWxlXOjxsjVBfUib0CH%2BVl%2FMYfgvk642BCjdGz9ZFw4vwzvyQV8zK95pXHdvl3GgCQGwQ2Xhc7U8EzMVbMUOphKHntVWaSSvG%2F3VGwyvaFXq7PurvhGyNJ0OWcWz9auq%2BsMTLoIXtMpXy0NVgq%2Fyu7dX0NgxJjMgKgrgWevhhhVtNk9n%2BH0fn5w7HjW6G9fpfzyl5FrBlFDbe4eFfvfOUnt19Ck26rA%2B0Ea%2F9rRCzzq9dhvaRWncmAOSubZnN%2Fme8K4X%2B1Gf%2FQVKJyNO%2FXZ7AiQ7H1X3QVOc3Ae9e1Ga749jPSYUNM%2BplPBe418A%2FYkReHt%2FWBrPLH5lNHsBcQkGAib8kOyl3YkRJ0Y%2FMSTELcf2%2F5MbtLV0I28tvOOua5AYaKp6QIvppLMwOMaiFg%2FzgGo3Rcar2uK4fPu9X0NH416nS2hnhOndiYQDinBUm4QlyaWJ%2BLgiwvJ6vF4znInrKpNYMM9UhcW20xGh%2BIXN4pfmEIkVTxXVmIcASNQ4Wiy4HCV7Ss5ehBuovlpVJ1%2Fn%2BFF%2FuUpqL7JtxNvxU2DDOySgTio9JM9mDDW%2BduVy6MLnDs8gGOqUBlHaaa40nZWeePFa7ZTxJt47y7vPEHACvXKyOEKOqpZDCKWyqnR9jj3z8Q%2Fqml2454ARgXdoqa1w2BCOzhG2o0qcGqQR8i6DxNsUqeG4FFxxHoXFvoC41lTvauYuwZCFxam2xsz0HKFrq62OLFOBpyQ2rhfvkyfi27L1o3toROcVZdFI%2BNfg8kKpQIl%2BVN2ay%2Ft%2BcUqSlbveivJvCsyJOiX9%2BFrq%2F&X-Amz-Signature=f6dba87c85842c1d9a6d0262c99ca74ef71893bb5c31fa4ff4be921934d784de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

