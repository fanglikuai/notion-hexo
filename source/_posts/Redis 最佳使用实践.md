---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VBY675YZ%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDmbSX34bG%2FY0KxIUaWpi38bbRyPGMN9lEqGD2VzrXxDQIhAI8NTBPDo%2F2qYR3lhCjY2uh4QV%2BIbp75ZWkIuiI40S2bKv8DCEoQABoMNjM3NDIzMTgzODA1IgxJJEy2STtv8NRAz2cq3ANuI91DMgn11lRV%2FOWJaUzs0Rr8N%2BStxqdPyLXUWqsDotEDdUNFjvyYoSAZT4%2BhMUAXnW9Nd52H4SkaXowJReZf3MKODuTAE4SqJSW2GFPRl62EYNldmt9TBHdr7d0rm2j3XAqbAY85jZxNrG07bXmKYLunIu2seIfD9CjKAqaJPwKBwGNg24U5zxkG4Rf5OF%2B7QCC995aTaf0kwhATxYMAkJ4VuyqDbv9s1yu4JmdOw152%2BVcYlIREMLjc%2FloSp3TgyEkGh0zEnThjp6XxEQj87GEvaNx12ETPiMAl%2F%2FZnbq0B6Iqo%2BSAkaWTvk1DD7Ybzah76AD01AORXt0jQAHLJGO4Pd%2B8h4JWvWoesJjgVJuwQF2%2FovcoxA%2B7fDDwVwitR%2BVs6wexb7FoYvyFL6EmhUBHkLDKA2i%2BETeJMvCyUKZGABaIjGfnsgOhg0NmX0cfbrnetGtTFaX3sSVBwEv%2BSziSbHaYsGAbO9hIVgDuuA6RUCNjtr7Wta9qd1LjwPjFGzK%2F9JQ4EoZVAqOJwTEXRHXBzo1Bwf2Xacq%2F6dR5QjvlTcmyugNZ6mZULC5M7ZQswTTNUG0dStkDSMj9CDJUUXAnevSZdHpN4579VoRmnKM7o%2Frlqyn2Buo%2Fr0TDSmsvGBjqkASNUovOtCu9K4dkFM2t06bL552s8RRwSGnqupOoNuxZLhFZ%2FPrxt%2FfAu79%2BlMiLRj%2FVnuOuTiK1FX5%2F6dzjQMw3ngCDLRT8nHhu%2FOrW74H7C3wqykIKsyC00bTM1FXn2DvGg%2BaKgOUfGOSq%2BKc%2F%2BTCTPsim5WYfQTNbOdgIEOeA%2BJYJ5tnEzx7gDeMynNvnmKw0iKvPdrLT6d8Q4aWXGdvehv3K%2F&X-Amz-Signature=83588aa86f22e54fa73e58fb365535e486ee6b1295aee0dc4bac691f42f0a9bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

