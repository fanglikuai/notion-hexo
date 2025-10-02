---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROFCBIJI%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDSTQ9aM3f4M0JXEO4Y%2B95uTlxCATrdrgPZlQgM1xtcMAIgSQI5kZJvn8SFeHr1S4WM3nb5Yi71AtOI4MrxQkQ0Yuwq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDHMulOPTQhLi7x%2BegSrcAyQspAeD601F8yEhnC4mmFcVC4kT4sIV9DtW4nCchbCGRqlRtxpvvqMrdKNqHQ9AuoOfUDMos4o5BZvlW9BfNU1V%2BXrDaQesFPVE9NubKlKo8sfW43iRfoqsqm%2FrUNdmci1PCmnOjXlKy8xeCYforagt5XTFottJSMCiAer8k%2BxK40Y3JEN5CiEeklEbEm%2BZ1NH3oyJ5ZQRpEtweiNj65juIgu1TISSXPgMkDT93ynJSkMrnoCNvkYWvCB8q4yxnYwsyXWnDisQX98HCenxI27EbMNSh6z%2FoOPDfSu2ddtMNMGsIQqGFyeDo%2FaCxcmXH1ZIvzJk5RYSCFK7ZSTaSvjqxj6H0h0Xbf4M%2F0GTf0kouD7rchgDN81hh7foNbmieHP%2BoPCuUlCUVnpNgKpygTdY7Ngiww0QarGGDxBw1ujOzsTpvB9pWceFxtJRw%2BRhZ67u8Hf%2Bdmw3QsRca%2B5g%2B39ZpYTKWdtEKyGUff7vdD2wVqU8wE8FkmWVBHz3xutbNpAKifpHeb4%2FQP8it1S6VhciZZGXecIbsUxNwoYx3yfEJqOoFgzBElKSxPS8zvQciikZNuw4tt4xyptkxtXFMqX4QqXBld%2FV5X8F3VPKOL6hnvORlx1dwUfG72qnFMPms%2BsYGOqUBryHE2wPsAeZ07ghnv4JEVApN2Cbvqcm%2FsZ1Bba7Bb1pm8N3E6lElXkIhay%2FmqPEZwUTGtF7huXhPAm%2FEuZxYoR2WsbSs9bym%2FF587exEX6gE006F9lbiiUhTFHhQIq8rvAPfD0j1e3mO5zujCJ5sS%2FQ%2BQeIA7YsC%2F9WQDDqILXqA73hpQ5o1uJDNaJrFhgJvQr%2FpL4umpd05d5xS7A1MbnodNQ0l&X-Amz-Signature=c04bee3151e304c4e1d4d73d9563f009d641b26d6117b75f06f1b45636de3a22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

