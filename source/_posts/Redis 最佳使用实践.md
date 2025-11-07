---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WAJCFXQT%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBVCh4URjPNB3cxVBDzFpZNK4rfzmSS8FQ770jDvyzD8AiEAs3%2Bq9hM92L4sav7sU%2BCk6loHXTXI2NPzN8EMJr35SKcqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2F6iswO0Cs6FxUaMircAxTRcoBa4PucF1ljaQTOI4IgnTxtA3JXT7Zdp1ih4X6nBqCo82H81gqipip%2Fo2ZYLtxUXh2Hq7Oj0YgVCP%2BFSlXgTLFv96mHB5R3KvtMFRaNUAC7Zic7b40y6Rm%2B4CHd8VcasdXOx0dHmGiO7bOu5tlxp363Kv5w77Qhze7IeUTpRDPIq0TOOGc6enmuc2YdaF%2BEqmQNnDRvUbkss8yd4dCloSWkFfftjiu4P9Bwwbhw8x0rxoyVntjUMATnOMQkzbA3KUC7JlnXl%2FuclUc0zWa407fOekEhrOmX0DdaY%2BQJHp9Jc8Itwb9AmsdkOHbMuzZRnCoqKTXSoDarR7oZvq9pTOHG7t0JjDA2S%2BpwVcijkNNg5w4tUshGwOzOJHuzjlUlWplV%2Bp0Ws4DQezNfAe8VK6FpS4l2A0Z%2BPrqKOs7DkbWoYrF2iUi2pQNOFAEB2G9n9s%2FuJG8vzCWgGVIIklDDLYepUrg6cHWjSL%2Bx0ZHM2CnT0jnPGia%2FFleBFc%2BmUG5dtU3oltOncmB5MiDhl0DJFgOSHO1vshP2G7IUEZWNCUzXa8LQMZQOemmL91AiGmGDWYa0gapilb26SDhsxpiRj5RJi85L8CduKWslfphYZg4g%2BZP71iIbkPX%2FMJ6ouMgGOqUBdkO8LDFtBacqHGq5fgQf8NyKg36CutHyRkTc30FsvLdrOyZk1yM7UAJc1iS6qyVSfU6SVZlzN6D3h6RAwaQMTBXe%2Fx4br9umiSApKt92HX34OgoGLZXJN7A25QPl4o1eNQ5U8hkDnLUiZv5YQ6tmYMlYFA4nerOAoOd8A7Adwm73PU2gWejTzpctU8ew0qG3PqAGDaGDkjHlE7LejIcIbo%2FLqcaF&X-Amz-Signature=f21dea552a3c0a9597732fddfbb5db83b9c3588d205c348a945ef8cb360d59a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

