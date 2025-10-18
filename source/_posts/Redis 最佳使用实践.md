---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSITP3OY%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIBNSlvk09gDKwJTWTIV1LcvyUsw4CNjlzc419nYYKXNrAiEAmVYBSWTKMUoLAbKvAmQAZuYbs3GS52GP0KJ4jLSQG0UqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHLZMB%2F%2Fj%2FLLYcanMSrcA7FXdNCTajlLyARQpIO5vTfI6eFmLC2wOcDfpCpuWZRKSaKsV14XAL63mTXYM%2FX5XGj3AgeFvIY%2B5W8qXFWRw9YvyWczS%2BTCFXQb0bCz9kyxzJrgShMxcIyAnpx0gSdIK0jvtIbQEWUrCoerzqA06JEdBMxWLgwFHmIU0zjSBuoHtVffLdYl6J2ntfl%2BBpKMd9MTuii73e1N%2BRLLdftgXMuSRrTsAaZ0aBZRggJu2e7%2BBNzm4dMjJcHkN3dv%2B5eGzGTKzVW7Mn6XZ4BpR9B710fD1r6s4xH1R9EvQA7WAYHzewd%2BIcUfjzhGdkHMvoCDpP87%2FAq9qdNfJuUIvIDEO8B5iLmxQTYa5KJIMABttIt5LetCcfqTgeESBYPg%2FTBg%2ByK2lbbFsHVTomjLYJklGl0T%2BdNx63BdRFzLFiuiZdiZ%2FoJlIw5X%2BgXSHiCc4iFgoRr6SNmEeUcx%2BSuBmU64GJpazBv%2FM%2FYgJ36JxnnnLLm9P64vTld8PFNJnUaSpiM2jg5P5ehOo945rGcS1bkAPRA%2BpyUubcy6xTQIXVr3Gbn0Spht1EkVcBtbgtSbpRO2oLydTN3Q0cBQ6liHlCmEX65Z%2FFgvzUzHdLAxF5V9%2BZ0wqTANx7KI%2FOsF7k7QMLSCzMcGOqUBT%2Bn0uOC7F7FbEXV3ob4TJAFx54XZ1ey0WzlyCLhhzqDdEfhJ%2FjUEcG83AlDHi2Ne%2F2ParijW5hwKppiQAFqLXSpHL05Dw%2Bm2JqbC8UIVX8NvEWTkEcrNEVRq36B1Vv0qSBcTkUfwuV1mfNwGcwn0iPcxJPp%2BUJV5VUJSCK%2BfYlhQkBFbDjIB77jC%2FKPpSjrc0CBKZ%2FzYpRf1rbOs3P88eupzF%2Bga&X-Amz-Signature=51585d7e3e2fd3695797650e7f937e4db6f9d317307d559b16f9bb0792ac8adf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

