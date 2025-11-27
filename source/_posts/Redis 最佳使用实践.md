---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKFDOVZL%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNvMK851TxZCAoHRCiFp0jFJak6g%2FXY3c8MdcgAYvHSAiEAxSjJ6%2BoXd8Ya8W0zMWJn6to1BGE8L3gP4WShqqbLbpkqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPgEnhApzMQ8bVUg1ircA5ApVTX4eLcYTtl9yiGV2lzytRTeLuVB2guybVgPGybPpAvjuDvyfJcRpQAm0P5%2FiylQ4kXQcUrbDpLOHl60RewrlfBLi57Xz90hfl1wo5h03X3ZSks21befhP5YRdzaogxX8UkZKs3F9W4mBDhuNgC%2F1Aj3DfpmnjhYmveQ6KorkUa%2FqZrwcyHJyD9fpvlH0bRy0738pDiTE9ZiEEk3pJfFzXtm%2FcqXscU%2B9c7Ss594%2BfyJFs6tADRMn%2BnHZeAwxbu0ReMWiK0mle3426TRBOKXljNcnvLa6RBMmUwrHbPHdVek%2F7iXnQ8Mpq49lkkAwnVOHf4iby4QT4SetCi%2F8sjWWOKsblE1OMtCq0QjJE0eFheT2nHO8jMEe5sg8%2FB28%2F0%2FhnarLIGachKj1hGe3JP34%2Bg1hzkLsp8A1Kne8sX8hZJ%2FM83hPWCofoQpACCk2YiPN1MdJgRoDy3fJO1z%2BJf80SBE4gLUJhGMzEUfmpAYvXUrOwIMhfEBqWsT5rh%2FzmEbe1hma4FqyTnuQ1yA6o4VzTDGSil7nYRmaI8a70KjoGrPE%2BRjhZ5g%2FNqhgjzNfLqvWAjyQwyloCsqL2J6HGHfeMIdnV40T6C4QHvTMXZsUWIO8nXgrf2Uj2sYML3Zn8kGOqUB%2BHPWEe47LCb9CF60OggtorHqQKHv1ug6%2FN2p17layCfsFt3yBSzzITCOr64qNnZ52SDNMmA3e6SAaHuoc0ljMyIFahMv1qgFnblslHSVOj8SV6IFVydNO3CpAt%2BlGasP8UEVGHK1DRK36ntn00n6ErBg0MHiGKQ5We63PsWIAilqza73WiiVlKLD6fujyE%2FQIu7HIV7fyX2No14OmJuVg9uFXIJe&X-Amz-Signature=998cea9586ba303d795c8d73a2d747eb3f119780a10cecfc79e496b5c327610d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

