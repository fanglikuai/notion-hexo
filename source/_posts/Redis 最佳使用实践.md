---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4QEBWQT%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T140116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJHMEUCIHQtVf94nv970bAyTl%2Bh3A8Voe0A4Y1SGs4DQFLB2xxQAiEAwT4xE981adUGRbTTsmweL%2FUDrMpG5Nt4gsEWCFZAtbkq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDN6Rk62VPqAovQv9PyrcA7no%2BFjPpzQszJEliPg7e%2ByE1C5OJxgwak6tGChEkBzMVGOp1ZPQXr7cQ2D8gmDtl2QLRF4prEunmSMoQA7Zw1jqHjlgmYt3IeO51weX82up0pO405%2FvrRp%2BrhFNC78H5Z1%2F1uwiQQoHvX3GLCaVZf9h67YU7lLPt6gY7raEYa4gA6Nzd2gktrVO8GeJL8lz2Oda3EAUQsbCDjUs67tncDnr8zg4ybpZIOwEN%2BEmlajLvtx8YJsiV5QyMBxcQk1YhHLE0OmpkNKZbLrA1KF9XZByHXKVsELN0iqZoe5MUFCITU1vYZwVjd21zmFO7NffdbZswPK%2BuAeDEtdP3lkhHvalHIGkrAc%2BVB28tkoSVzrS4WPdHPFHDRqxjxgoFxZodilBUeCVNQGdMUYdtHpQud4badjW3%2FtlWBFSt8C6tntfGvC7mFuiSRfXVJhnYt1gWu04usGDCXGMPbBRMOkymtVeEwALGK7CSfQCwHhjsDtntORf%2BZ9kPJLXtUwoxsv51K6LDHOAWUx9ewV32PlWTBtmVASnSrPOowJZwHbp5Wbz5fhdUX%2Fqf%2FHOzlreo9rA%2FUS8ZXo%2FRodDwftXoFxAtSv5JvHB0dGBFEtZUQTvxvYv5tXTr98K4eJi2r%2BtMNCm48cGOqUBe17cak8Jg5jUbVOne54FhBA7Eff24foA8DYCm9o%2FcCR9MpchvvVYcgd0XGPUqKL4%2BM2YzJv5SUqaSOkDNoGBOq6XV7fOtMLS5JP2i9oUaSOnDVOfKZpWX6IEAnZqOhrjL%2Bg%2F04fThW9bDFCKd%2Fi5CbY5xjOZiDeEmknhjKFvqWYLvU9gdq7dfcmPGEdH8ldVRWiwFD3BE%2Bf1pVMnNbUHWRVJEdLr&X-Amz-Signature=2542bc90289af150ad92eb60fca0b5cd06fcfbae325e93c6ab05d77b822532cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

