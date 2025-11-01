---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VHMN7VZ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDCggtv5zDkDV0bA8Z5RkLMEm%2FQKexncE6swtvq8bmaJwIhAJXprM9MtF0BTUKTIadyolOYS4F4i%2Fwq8zHnlI7NTLM1Kv8DCDUQABoMNjM3NDIzMTgzODA1Igxj3w3aM5IJhzJmGg4q3AOP3L%2FBAo5zeGUMwTQIxowtoi8oj%2B98zDS3dtc5h2StVLwLgHpgXizmAkHpvZYyfGICSmEoh%2BAOBP2dhmfCYsNfQ2PKq4JAvIGtFppGydJBdafcVOBKu%2F2xKgFFGi22asnbngA%2B6Q4Svf3nK6CyMzt3JAEnVpxlwVGO9LREDIdvj5g56P7N8dTDJZIFZIT9DvIgtlhf9%2Fr10f57VTWktCCu65Ig9TXbsxwMVbuNR77nCbwI3aD%2FbEsQCEQtGZiSjznKnUINC65UEU8sKz%2FHvMBWi8xUlnItZGbEKpmw3xotdEktOe2GExnPeWLTNyNuQQ%2FewBH68cvsbwV0Om8S3YEhsgvebPfVop8G7oNkjtJAiLp1%2BygiDqrSDkWvRkRtColuWpbWtl8aaiolu1lWYrpoatB8Qsj2On%2Bben8Scxe7imTEJB%2BDykAoRn%2BKjszUv54TS8Bba07oCzjRfikObhzLngQMIuf8Dgo9qkwt9UvpQy%2FZTehfxRhSgoQCo95qACktnq4U3DTS3N2eOjYtKEqf0PrLZRExbeAljT%2B%2BTUYCqHxM6W8ADl1rRgJPuTF9SmSMmKw475UwR8Coos%2F6cxe3YuLkY0cHibSpUnAQ1%2BbRT1bSZreZMcmn8j%2F2RDDJw5nIBjqkAaqMRNWSB8JSpmQ79nT5IgFIz0Cw1ZtVMwJxGQzc34pEPvc7%2BAFM35gpWyEU1OCprcMHsQUmjlZy9Il71wmrKMy382vFfDuRhgmqufeLNvLLojScCKANSA%2FpOrrXGShGBE8qHjvWz1MAwtGJfa%2BJ9tFNg1RLJwbDACVTnjqIAyz%2FfjFDC3vEx3lnE7%2FAhZJu%2FRHmRd1YCvbn%2BSXWSg1Z4A3tarua&X-Amz-Signature=d259606a73ab0e3266613395ffc3847a3afa34fda5880aae2edfa3a38d10058d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

