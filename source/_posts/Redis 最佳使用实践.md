---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVUPOMMA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIEd97A%2BluO240HGX6lO6aBCSprSCWKE6iS%2BFT7S%2F60HEAiAI2SNTHts6kcx8b%2Fj5T24jVQ%2FgWHPUA8IKLHhdQIz%2FxCqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlFX%2BQOamDiUSQmGbKtwD82mF%2FJoPwEU%2Fgvo7IAl4fEyGrT3kn4mNWD1%2BMlgPe1gPht%2FrR8%2BkySap931dZGSu8IglM9zkx2dTsZDqRy2uA9%2FgiUX749Birmd5ZAz%2BM9XjfVAva8dp2Dez52MoUPhHSmIAA5JPv8puOCCFSba6DqjuW%2BCcnVRLayYEnxUf1JXqeUCxdf2r%2BaP%2BrG1me5MSPOxgf7GeJzKtS%2FUkObCgef8iO5Z6xpjL1DT%2BF3PdJk4XFXDLsD%2FqEScD7E9yd1QDfB5SEm%2BTVmQbQccUxwsZgI2tHdrQhzBb0zP8jVsb6Z6tfgeln6OjhJ1VNtCxbTGTsqisYR5doLzq9Lgx9yIa08v45Vd1gDnAqbSNnpTjkHRjEFUfbyV6h4YB7X5YfAsgeaW2%2FX5RKdD2Tg7InxfTOLAgP55rJVC4BvE6u%2BHwC%2BK0%2BlqAePHmrN0w8ZUcjgr7AOReWctvTl3d6R23sGfTpDCUMySFPKc6pommRz4ehW7hhtu3DYIQNDQj01aIwfIn8ko0P7KyfrFclurxjgTz9tM2JL%2F5QCqtNcrkXtH59fhTuIiS3GmqbcRkgVNIWdJ3sJ25FrcQNVV93FP4V0DYzn85x1x8k4Ss7dnmIcZvLK3H5Dlj3lm0wJsc%2FBEwmK7CyAY6pgHdseNyaQ4ghKgAI0CaYgvLxpzRwE9Qq%2FQxwhtBaZDF8PpsTZc42iThZ7GG%2FpNjE6%2BkvsHvXYSBaiWzi8DCPsvaFh5bfFZNFrgZbPcBZBoUBOBHNIYMwMKH9yHKPFH4I5TD3VucAQzGaSN2mWJi7TeaSsR%2Bu67FScuY9Vlt7fc20XELJ8zGV%2Bb4tphZDK17mpnNohDt5Nw2ds%2BitcdjeCk4jbGH7hwm&X-Amz-Signature=fd0b2b2194775e8bd068162a486e89950007601d1ae3669180925aec4207664c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

