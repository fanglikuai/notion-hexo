---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IEY2PKH%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFqP2L42%2BT9COB%2BT0b8pY%2BvEJsEkYTccOdO4rVWTqIg0AiAaTzyolR6r%2BBHQ3qZTV7N%2BTMFZPCg1djhs%2BYqBmw%2BPhCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMyG6pDGX4a2hVh7KkKtwDGGzRDa98dJS7C%2B3AwOlfPF0iSsCjgbDi9m1KZHd2GSiBCGpkn0LPPwEs9QtO6nDz75Iv0%2B%2FzHjSkml%2FDphBJObTT8vBSNkZ1CY5qppvp74EBQJpLOFeGfPQ1j9KYVoQLbHYRW2uBnJwmYWDczn4QC1AQKsKUx9IqHoyMKS7Pl65JtjhGCUPXjzgQnplFrhTVbwG6sTnqwFE5plYZew%2FmJ4LTN7M7%2F4oYs8N2SaiiuX%2FeyijTplye7YozklZlBePew6wyaXC4ep%2FgJBvzMKp9sBcm2gHimXplcj%2FFWyINXiOsowO6Dg4lIvPIabaDdkaEUwmMnTt1J73D5lal%2FtcXn0Mh7Ps5SwydAmi0dz3txPGo4HypPDJceBucqUdcRPmvrwEAacEoRIktwzya6hbuIClsjyF9A3IHxCmBXbDnRD6tSYcSOXGo4g0qgvx0irg8BIYCQA3nNRuChBCtRcINMMAqGhB4oCOuKtizYY9Ig5naImLuYAMiMFBRpi4t2zRMAz1EmRzLPgAkqJ%2BzAzThM0oYNq5eAwx6WFJDS7i0ahpWIdzoWsQUNFdjel4E3OU7glu6KsdGB59z%2BqyYN0Rks%2Bpf3JAvYo8jEyalDreDdAbB8f5xjv9kPUQTlSswpNPeyAY6pgGggkYAxJx56QsD1SawE2JrRjENHDVbd9gBbJb2wS5N8%2F11PdlBaiRcNPU1OrP8E%2FMatges6AsMLYhDOZjqu7ccCUgtFkZuR3T6D%2FNnrRfEDkLLWae4p%2BhlOdzuqTxjrqn47KQehkSOjpD8f8OXcshJmbTFTjCR363opmYbFC3p7%2FdIBHQQrNq8r6z1P9dwyJ23o93bpFPsL7aPXiiqSNiBb3wh3CBs&X-Amz-Signature=99a72932b7e55bb6d502103c303dc26147523490c09c8eb9c638d3da975b3fc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

