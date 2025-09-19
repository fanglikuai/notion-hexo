---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWATQHQB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCGeX5lf7WYIiclP2PaDT%2FCoUOoQfHY%2F%2BHHjvk7d31p1QIgHEsXtm%2BBm2HLJ%2FfPtlL086CTtkPU5wgWeneApA2Zo3wqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEAcasX%2Ftnc4OPExQCrcA8anrEcM9imVEZUpFk7AcDD70xEwdqSDNCzqiaV%2BwcxsZO5Ul2Exx7saPrlQhM4maQNe6tIDSiloBLOcMUIF0dOsgG2pFEGuEic5T4bfaJZZyhdO1E0kt7pMQKFKSUlU81rghJTaP8I3cHRNbG%2BM9AVFqAi%2FCBBC9ulE13I3oJdSyhlQ1fBPfAwLzfM617dtEYiNhcUQO7k39eTpdUUEQM%2F8bHeT4o975KxvXM4%2B3D3m29z79IXoPEBlP4KNxLgFQxIdkx3LNZ3LOW5qb6CaVQszD%2F5iXl2rmVB1f%2B5SEcsJiWN%2BQvJsHNg%2B4XTsXmZwKr2GfKa8ps4Frc4%2FwlpDWcHfakgnPJpGP160pxdFaJUUCDD0T9ZVTVfbbx1w6Jy%2BUnZWf2UXWPiasS3rvf2BsoIUY9AOuM1DIFuwFzYGBS0kEWVHD1u2kzoym2461YeML3xJjCuLLBUQuMXvu%2Fibx9zU4mK6GRpRUnDBnv%2F5p377bk%2BAfBn2262hqVcCvjDeTzCDQg3ZrgSynqurCY%2FI3FpmjhuYSdYKBq7CsjQpoHJZrVMcWa3qe%2Bz%2FvoV0Y4yEScLmTDHevt%2FWj3%2FU4NKzqQdWkPLGn78wygTWSpI%2BP8dYOeAUJFk2EE97fYh1MK%2B9s8YGOqUBK7c%2BSP2j71v0Wz0l%2Bka1P%2FhcYWUz7hHpw1SBm1o2d%2Ftb8ixpYT8r%2BCpAjd7JxszwvBmsRcC871U2kwh%2B9T2N7pS0YU5TcmQQvfwsp1Kg5FR4YUn34NZ1XHc6PNiMJpKC4eGpIxEFJ3APOSLPu7Mqdyd9HO5semT969Y0KVm6jk%2BfX%2FznQUFsD41ooYo%2BKJOch%2FY5CDkGaP%2BWTli%2BhQ2OiVKGADp5&X-Amz-Signature=5d99239c9383b05da8d8eb467221c004966d528883ba95e8dcb54510cf471250&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

