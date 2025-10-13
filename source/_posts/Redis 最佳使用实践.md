---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SH3CWWMW%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T070114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCgP45uYdjnAiadIk%2FPmlswAgXnFoZNtRiWZSVYvzmGGAIhAP2bhXOE7HJwM67VqhJdlO4zKgd3baJ952yJc%2FRaOpw6Kv8DCEAQABoMNjM3NDIzMTgzODA1IgyiWD5bsNWL6C4Yl8cq3AMt%2BDDY%2FtbbqnsY0rM4in81PKAjfuM9DWFCCVYrIr%2FX9rig%2BlGI7jqfSTSwb0MO%2FmDL%2Bx%2FPr3AKoaoL9WVT5vO9tDqIbcwBSWDtwjwmEgHFByYq2y541lk06xHqZsXcXErKVM0uVMcPYB%2FOexdmbgoy1SvFxKbP%2BdXB%2B3Jlw%2Ftpo31arSoEQIwXM82s0F7gwScHHWRrdzWsieZY7iVRxZVOxIeXBTBw%2F4jYg1iN8vdliZO3CeR7OkpLcj1IuPTSIOYLr1SvElBwiUj5ScAVyKlM4iBg4uQaMCKv8muifVx44nZyoEOIdClMfQ0GSIcVudd1jZGGIv2D7aJ%2FEjtMUkEMd3hOZxcseQl0Qors1%2FwzttlIj04%2F23I%2FYjfksTmVo%2B03171KLwsSUW4KF7whxqfkJ8njabxdYgNULLZwiDT0Kb1%2Fo42Fz7Tna1y2d%2FWzewNdyVv%2BtCzEBkl7891HEiZWYgOYszF6CGqMONMN7pL2zB9GNtcemNagoLvBwHzdF4r%2FImfZT1BvKQxuL00cVhtpJOi5hUY%2FvkQWjrhmqCkzW4%2Fp66j8tk3yu%2Bn1J%2ByYd0rW4nGoiCvqOECKclWPoTF6EAhqlMUy97Q452i4AW8d2%2BQNVnU2%2BS5sk%2BWB0DDivbLHBjqkAZm8ldhhUkXceG0bLUrTvdm1fL6aMK5vvuR9rFj63n1hv6GcBYK4Ow3WtRkjuj8pI10vgEWD%2Fslcdp0SPvyatOKIDpZ1sQ1lq3dDqOUiZf0YfW9r1MAkYQ5iA76P0g0MZYf0U6LBHBg0FkKEEEpfl%2F9wkPFOOfcHe1AjrW%2F%2F0fumokBUMR%2BFh2sCV5EwLOc%2FsIcUqZregMjBLs5XrCE7NeCMjZoO&X-Amz-Signature=8d8b3240b1c79a4042462b58309dcd289ce3095b2f2e18a1c106fab4537108fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

