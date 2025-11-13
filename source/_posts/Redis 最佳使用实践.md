---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3YCIV4%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNDJBrtmWB25SrvmM0rV8sv2R8%2Bn51J%2FkFEnchHVie9QIhAIqjZQvI4xrXVtjXIkngwN7Gdfx9OurQCXvSmZCaD1A0Kv8DCEoQABoMNjM3NDIzMTgzODA1IgzBL46mgqbaO5SIvyoq3AOIK%2FjF0OhHzjrlYPOJC9ebNrBbgQBtJWmaqg7Z%2FbbnDRS8FtDk2pIvBgmt0FcWzSOn266FgR9m7WVmHQeBmK3c78rFNPY%2FiMzFAvTVCabHDDpnYUJ5cZBUI2ANp2a%2BTg5B9UkYtLLKC7rv60HbMOWo3Veq3VEEW63Z7DFGnJm2G8EvpRgm3TehWiaI6GWID41XadZFFcEL1oJvGIBlLaMalp6RkWpTBlpVNWH%2FDV4WTwEEHjr%2Bz5JQaYzNXaSClDM0TMiyNMlN3OLE2ha9GrSspCp5pPKZoiKzQ3JHdW6McYFacksyFx5Tmbo068PQRird3trUIh3dPoQ%2FHIvHBp9xSHWBNQuULyPaWhuB0MrpiLxRuiWaUu56%2FDdzmAnPsU1Vk93rjBt0GRFyAZY%2BdsdNwxANj2oI%2FsjtOJxjDuDWbb6vWy9uWyMZziW5yabXowJfODBlC3mFnyTMBTkSRP1iODWGunbmma7i2Y7pylCo0n%2B46%2B0TOGmPBXXH%2F6yr4VsCOA7Os%2BgasnZnISMpFD46AfJvJAnneRtRmJSfM19AJc6hk45R8YYBG%2FTLZzmKHI9I5olo%2FbvC3GRrS%2FJ3LY48pqPBc9uwYVIrgWiEk8A0A3P%2BFkIuDyRW1%2FmW6jC3w9bIBjqkAUltBoLtxwZcw6RR%2BcoXAbaHfmlaQqm6v%2BQ8sglAuGuVcY8IKfc1AtoHuRtnQM7DmtK7DlLvz%2BAj6heQCa1P9RMOu8QL9KHI9ly6G4eGx0uf7Y5LIG4s1D6z%2Bjde%2BUQcQo2lAE5r4%2Flu5gxaUdbMs1Ju%2FoHa3j%2F3fUKvXOYfmrMfunO%2BqJL%2BqDkJiDrYf5HIdjiUrmKw%2FNAD9ljE1NKzIFaRaM3H&X-Amz-Signature=556e9f31ae850f14cc6625e6647dd22ab0a7dced9668bdea4dfb926b441d42c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

