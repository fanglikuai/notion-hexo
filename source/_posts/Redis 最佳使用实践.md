---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635ZPRZM2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTlIljWGm82KvvYZu3XVPf%2FOkfpSBbGa9YEOIrGvn%2BxwIhAK9oWhxzvZXpR%2FVC2ZeuS%2Bqj%2FgrzmBIzu%2FG6pIqBAZ30Kv8DCGsQABoMNjM3NDIzMTgzODA1IgyMlTA3Dpz9c2Sz8FYq3AM%2BwJPRqiy%2F4pfQF1EHqy0eoSi8VcRan5gmcPGVwYPcDoE%2FGejf%2BqhFeJF3wM5g3DGUvr8ZojDYSpFlMvyeqN8EkiMFV6okkvUgOUdpJdi0C3T9sWTBoKeDERnlRM55xnMxrD%2BrurnzTfZ1sPaIZnoCicPnx6A9FKH8X2W3BLt%2FNr82gtDrJGyfV7l8rK%2Bmy9VLPFowf78iM8n45PucQbdzGeI7%2FYoAYRer0AOd%2Fv%2FY8ksKrPHoG5PNZc6txqc6dX%2FU92iYst1WN5EI0lGYWENR%2BWrbTtun1TDYVdEpfyj7DEs93bpVpwqN7%2B3NYyVEXOEXM3MBnUgreoEHh9e%2FT8uGVuk4zXlsDg%2FQ4luWnkjCx06eWVaX9z%2BPNrU8r1SoKGqhhWAX8zC9L0x1hLSzj9%2BTTcVi22TqjcLCj4NlSAlOB6066dzi7kgiYSvm9idiR%2B3UJdZtBPGsQkENaetorMGSXhJVtOXeAAV%2F%2B6wgfFKGzSQ6Ee1e4WHkqF7JlFbgX6qtdFLzmPOpn0uZ9awCPbf6xZexrxje3ovR5o1J0PMZoxHpDV2FbR82PP1ms%2F573akk%2ByC%2FdBkRVx8bLVcBntvh%2BMLBZHmI0%2FG4SeangHX6TwN0ZaOM660qmfK0UzCS0t3IBjqkATHn68RToIySPMgrjfHwVBltM2qRch1O%2FS85UUi4PG149nCrcHDr6c2EWLcLfVbhnMyQ4EeLd%2BQjm5evxWVJ%2Bz8BS8oEjFxTNhInzJcYJVqkPOCcUG2mzcZiNHahLXZGfLoeYhaPwRpGhidnDFgw71u6Q4Ed4N6tZnz7NKjeBrJO49GhRo4boIP18ExI8YDT74NV2Xd0y9CbhsF%2BTtKxOmgLGafh&X-Amz-Signature=fe09410523b90fd6bd0e9ebabcf63a5b872d5f696f274533c38043024cff3505&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

