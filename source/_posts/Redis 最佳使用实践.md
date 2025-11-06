---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LXZBKQY%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD7JlhedcaUb2LqYRjBhtYcKGZ%2BX1%2FXp%2BXZnecLmV5%2F9AIhAL2MxA1K5%2F%2FuZN6Xk2Iz8wSY%2BJyVeV%2BvG9SwyD2lzWuwKogECLD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyPRo7BSLT5pBuWJQ0q3APu%2BfgfpjiYoldaXfu3UHrpbdXvKATJhcFukglNhUu4ecT3T5zf8GWFpkS2qoQjtFDl9JlsxSM6Ci24XNeRtto229w4xobHM1RWx%2BeKgcv4nr9igK5780I05buPogAF3m8PHTqzTOqpxUZnXKpDr7ThLTeCf6ZTazPDGQtLJNxyPWV07cerlWOQddAtCw5xnWLoUeXO2mOTEBYTmhnuRQ%2BAUSF2xqEhLxs3wR86bkmE6EGwqPaUrivhbmMH883UJqovZhjj43JZN1SzBh3liFe9gcVmEO%2F7AhmkzR7XBVssU9VIhTMyujd4FoFWIWhmjKrmAjpfvFaxXqaRmmyuMtPxMVTq04%2BiG3kFh%2Fr0bbF9QC%2BhjwKVCGDCVPn9fzQiC8nkVJ0mo6mVnFGkjvG%2FpuuDEyka9dacmelgHLwD25m%2FTvlGs7duA8U%2B%2ByKeXlnWmLOgDL5FtA58EHxCwvI3tJjf3VgixWAHCMtK1LBAVYYnMbLfp21x9UaoTPPdiHbFd1PaJdw077RVAA8zVavNekGKGbtASKln35Aq49m1UaV9p4SHevn22Gs0jOTpmRqe5%2BEqkzr9FC%2B7KdPIillV9PSnKST8vfqfCPimUFPxBzy9EPdFnPByk5ZJ9mx%2F1zDKxLTIBjqkAZkZArA3NTgD8WN%2Fca3cwUEMJArKuBACP4U5%2BwkHHsPtWTTGJDqvoU9ttTs%2F42uEkbX0ePGWXkYy4bzezSDprmkDrOdGQmOFpK%2FQX6YUv4gFkmtlk7vMgLBANA0HnjlNOBZHuq4n4bDYSSM4g4L4PY%2FlCZD1tQeGnjaPKijIibicGWy9a%2BFr7nV44xUthWYYMWkyUwVvANtwMb4Mlv2BhVdmTfEX&X-Amz-Signature=64355950aa541e52b143f7800f7d08e19d4ef47e28e1256b786f0cc15ee5ea6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

