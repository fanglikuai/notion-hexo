---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674JZJJHG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQDz1jFbk2HPxZ3TnmrTdsm08Z4DeNVinIYuyVIuVTXWcwIhAJNzfaaxZoEhsHw3ddA4pZzONnspjDAZedhwQWaO8yniKogECO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxxCLPPzYWiR%2BHnx%2BEq3ANSzxtXNR9deGFPvMIuUy8JIPiQXT3y7NHal%2FpBtewDLYIykRA1F8lQJptUNCf7GprygeWv7R2o185s2vNhwzLuLP6igjEFouLvLub%2Bjdu3jYlIvf9n3Uwa1Qdui74dm4YUmJf%2BoHK7UiEXP8Tpw00YcitA%2F5Xt4v6pBTspgvfUIXoM5miV%2F72EM%2B23p3lZjqfK3NhNqaMlAtnQRb%2FLaeABxLFB6EXxq3LF6EYfjGEbkQKSrumx84erhjnQGkO1vIdryCqCrWYTdzqRvJahZdDNJ8E7MjUlM1E0wni39lv1axWaF9KxkyEjsVAroq2B5RdFTGilg0elgp4ZsewTU6uNgwqYx78wluMWJIb2Cgu%2FdYSitfohourE8bUyXZEtvfAyVA7uRhJW%2BVi8gt%2BgEcoLJ7paVt%2BFtZUy%2FyYt5RzWHwfn%2BVrWMAoMkOY2NBK%2FpZNrnEli%2FNEHQ%2Bgi9Esm5l%2FQSsm6QAaGRtwOnOpB%2FzcfmMcXOuN18AItCUbwYpTtzGV5xLcDkCy9GMV3X0ql6JO8IoWdyVM6z26rhYLRqBOvEKax3tFGPDBuji1QYf4zQUKGi6HcMyWJYA3CA7zru%2BslNsibzvF9L%2FJNSp1Hii0z0u38ie5z3akk9b2RnjDOy7rGBjqkAZ9FFm05hDmcqrTLbcHkAoJ6OtuOm5mb9y6JN2FKSnHDSzSXQbJTPK%2BgLRnGtUYndF%2BYGXI%2FfPsVOVJs1%2F%2BupkpvR4azRHSJyF9MSFamDjnMlXfnNfhsw66ZqBU%2FKSZxYuUjTHPlI8quPbxTI3tsS5SecSAj28lN828zuHkDatLW3rdb5R%2F%2BTM9QS2HbB6rKPkaW15kKUYgYDZMWIjRFN2u%2Ft2D2&X-Amz-Signature=e47b5e03b4f289ee5e0c55dc78694c6238337f9f9682ca4184ee4a784f90d663&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

