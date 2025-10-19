---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYCZQ3KY%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHLEfsEqsgT8Tn6AiGICjOWoRraZ6JnlUfqIuO3ohVunAiA6o0qNgUlohB51H7p%2B4NMa7QZwdOjJgT%2B10TM0RoO24iqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgb7R8LFo2%2FpVJceiKtwDEsHPH5swfTDasqy88FRPmTeYFBvudKs1sEdzTBA4t%2BYbCbjxEIce22BDkMRBKx8oTbyZ%2FXl9sDYelBLx5CWdG4Rj8CX2QtosrsOTg7KL%2BhSvGyCbBeHIJ6DC1BeficgX0dBQA40olskK6PTV%2BHq2UtVLpWczMuk5VVb54fMeAfTvz2GCWG8b6J72N%2FH2sTMpWqokQRg%2FbBJoK9naNDp1hEd%2FKFYcDfbbXcCR2%2FS5gkWB%2B9pa3293s7HKnV%2FJhXpp1DPiMeQRD51jZuJjLKGboMlXwobRtAbJd0mF%2FA5%2BjKPLTiOwfgCig%2BMB6RDN4sT87%2FIS%2B0R8F%2FzCp6QD7LlnnHhkF9AqqGUGyqMViGwrCciKaK8%2FW0KtEAv7br2yqiGFxushc0c%2BY44ANzmc0kQDfpURpP5TUi7UGGU0UwOyYkQXk%2FVeioX6LrsJPYA996FPy5Wo%2BZibEVojPY3Tclo8zTkdWm%2BQKYTjgSgpKq7uESHu7fDKJBgi7FWmM54%2BNLePJf5larIQajPqqa9WjZg%2FReqAoKFKYJtuSwhnIeWHZvIv5xl5y4inRfwrzmR7Gdyx80prV6MKs%2FEJ2ErWFHGtS5OD5S0GTjm1viL02Bk4JllA77kfNipiDLeP458w4tbUxwY6pgEST9sQsNrc3VMnyUPrnzqBEs8YtcLCB7F3DJg5D7YEqSDFW2filhykcNXciJE%2BBRI4z8JtfQb2COOWmgC3v8f5hbPxV11nI0dr%2FSPkOiJhC29EVzAZ00TV%2FFfjtpUgrcJ7F9FT8idjTA96GHvClbLhgeSKG1h3CiT4yVT1yZOO6h0WfrgbOC8nsxpMZZemxbT9%2Fb4WJoOGJFXKlqdSdY3es40asLMU&X-Amz-Signature=db4163d68551f28d9afd535d3dab28e228c19e9b62ceeb05564fd245b5c49ba0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

