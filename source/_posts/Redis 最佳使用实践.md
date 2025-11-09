---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWLDZ3VY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQC8OBK8Rl6rzrmd0nARsPgylXzkRep25U%2FBn4wOaQSDnwIhAJnpYvSEo8yE2Gv%2F3f%2Fp4pHq%2Fvr30CYQpIbY5%2FiCMAcFKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0pD%2BqSDRPqRSMW6wq3AOx0ljdgiuzzb2guxpcLLlB%2FuXi74ZrRZZ0m7o4YQwhatoXDPYd56yagcQ56v8xJasrfboAFc6kVwjYT6E%2Byw17idEac%2BGzlec81dhxOiTcZR00TUgWAN4a%2FIXvfvWnsa2F0PIsakPC5ejpFa3oOwN3WP%2BVJsI0rO8qcG0H8hyzEa8iauIvEJFrM8RoyOaoIRhEUcqyHF%2FAR4txg9UstbfJs%2BteAtGzY9yYONPsrmXw55T3i012PGPqU2TonyGTomyMfPO%2FF2sVgvB%2BV0TQ9T4jv981sq3EdJiKYBfkmS9aeC8E4ghi9CiPh0lRV2zaLkPcbacuNW2E8PIUvQSEKG9FjFcQwADnUxyOwP9cTTdwe%2Fgoanwj3gWvQERESlcxK4M9QDZeL%2Bhy11bpPIBK7DVXxqzW6eZMPlfTKQm62aa%2FE9PLfiV8F31hW8jBD7GpCEDPi%2FjTtN1B9ht%2Fw08tTsQRkudFIuMrvtlyEt8%2BnX94iZ3znr19zgGP5Imktq48XDL%2BevbY3txvbHHhutMwTf84v0FAvvqfuDhz1cnD6thOZ7uFcRe7UN3U6nmo%2FcCkGNgaaoq49uU5vr6RXEs0%2BgJ8taRkve5Av8zfUqCz3i52ZZF2DOB9zOYXW9bERDC9gcPIBjqkAQiNV8rdNF6zWYWp1XJusHnZ%2BjyXUTPSF3X02u%2BByH6pL3XnWa3sMUwUpM7gwBNnKj6yBi3SsaH9JI9bM1Z9YpVOo2DJuRy7np8Q0ZFzWupu1nrAIg4ier8w3LNRImCIo7TxzeGdjJBrphUhIQxMnJMVgNOknNvyrIHKaWfn3CjfHDUoAxKtl9C2EPkDQwhiSWggD2m3fA5abF7DcQHBsCY%2BJkBA&X-Amz-Signature=39acfc2235d1370d559c8a6485ccf8dbbb714a6f0df76e7388fff77b64bc6aa8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

