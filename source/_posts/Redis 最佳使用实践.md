---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXXHR2NJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIFgRhKFlk5Q0gDx4D%2FPPU5%2FSJTkMc9DrItT9fVanR3pXAiEA2BDyTIEinANyqAAcE56shyrP%2BRT4MbNTlDbmIEVfr6EqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwJMf1%2FT0TumRWE6yrcA2I9XoaPuNDypHOFDM4Q4jzEL4TCCDf5XSlWrM%2BX3UdxUrEb%2BrPgJO2%2BYXX1WIeT1h2wJTSjosT%2BbcNlxtv1yMas94CkguWi7hZ6edm%2B9grLLpEP5uyNDcyAK1XIyWS6PeMdMH2wTTWxW0URajllGosXAYr5lGeiYv57oj%2FLLNXo50%2BEiBLcdcUmBUitCdjzRGDT5YfJT17f6f9vooRDr10zYt3sifC1Sv5yEBwq3G%2BFUXfBNF%2BiGqfwN8Gi8jHyqzBB6i3jhMs9ac3oLYHlnQm8eeJhGuKLVZ%2BYJowA7HSmDsZld7EWIGYU5GMjWkEMEnpaIb%2B3Ul04cI%2BbLZoGr1m25lISbzUYuJxFGK4%2B4aniE9k4X8FHbkyFfaq8U4F14EkYbUSNmTNF1LO07COSXO3x4XrgeZuNLYUkD8Ayj69bjN8hDCg2Y2Chhm64MYcR0QRLtxM77Kgh3lyR94co1ZfTeJ0f1lfRqw04WVnQMMCavWC4CbooZp7Aa7%2Birfv5i1G%2FNEsiyttvxE2MfgHbx24UkmvWUoJiXoFJYzXdalJsKwUhMB1J6VGnyjAUX6R5yL2y0Oa%2F2UizIekypyRwta%2FntKCeg3Iij%2Fz0%2Fujx6h5zLU9%2BmSav2rFVrmlNMJua4sYGOqUBgma71E6KstftMR6HxZiix8%2BKScMDgqUQdcusAcbI8dC4yJDt8xie2guHKMIfoIf4sUT2mkVg6SAKitviHqk82eUlt4EW8DrAoy88zJRINy4kA8tvd%2FSaLDlQrcg%2BnIaAbR558S4z9sa3nZF1jjBl48HdtG3L%2BISJvjTkBJqvRoPjQHg225B2LKCBLJWm%2B%2FUjKXynC6z0eH%2Bs%2Fxh4Q%2FEByC9P9I1G&X-Amz-Signature=abbd4b950abb32b6dac6ef472c1786322d7d1135f43388a88040688a39bcc289&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

