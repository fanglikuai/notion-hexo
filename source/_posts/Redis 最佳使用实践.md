---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466REKTUSYA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCfSw%2FHwShz8U%2BQLgPPNKzNAdlnBn6AEzl2gI7kEqT0fQIhAIs3jBu%2F0k%2FbxPCaZri0o2KkLKDFScedEbjYZeIY6OvHKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwVkJvrXxNVUoR4okUq3AO0KMTXLFisZIVTFMx1xOh73%2B2QwjZssR%2F%2FtuNgstvDU%2B8wd%2Bbir%2BHQLBNP9q%2B9oYvw8dqQwog0fpRHvkZtmAtIt3fejJmN7hTa0nOdXqCi3k%2Bv0xxY%2Bt3nfHwiD4g5gmP32KB5BdQQ5ZYovZ%2Fi1xwANeIiRWgESNbkEa1QKJEuhew53%2BfAKRVg%2FLx7g2AZ0HtROQtXsTPrrukZsi4tbufgrZXCXN19e7bsorqf%2FF3Ac0ryMuxbV5a6fd1t3isTWQwJsBFMSLe6vEvopQ46jJ%2FzTgiL0Xm2s9QqtOG2Mvk7f1Kq8JGbcbdSqTTlnv9gQrczNsTdU4U5xx3lgF%2Bt05evBFHO%2Fkjm8QcqiVBxTjWPAvCetiONgFZF0g5oqs2Rz22a56SupnVziyCf1fptd25Jol%2FPaz93Mq4m6z7PxDAPyA4OuZ82%2FKhLJ%2FrvNByRwT%2FdkzxOmpEI9okJMnNXAU1toU5MFlGNfpyo1Fdrl8dUxFcN%2FP8T%2Fud%2B8edqYl2w1BEzAnBrfVL9H3qU6JAXQw6ifxyDPrUHNY14R%2BDHjHyq3OpIJaUfQvNYOX8a2R2ffpkK90KwoU28jEJSSiIp0LwPayHc7wNdSKQ2Af3GVPD9PJOy70bt9ABETIBy6zDX7r%2FIBjqkAQTt3Nlfu2XvLMhFdvmeFpDvEQEDZoMeveFgWfM0CWA2aYAaDDWJsTgwe7fSMsvcWjGyMMDMSVeYURKQV6Wzz%2BRiRPQrSIh7UilKxYSCzgRHWU71MpOW3iCUym8Hx4HldlDh12igWCFkkiFzSVIKMKKxfXTeeXDg0KrOvocWXBAxOcPUmBLJriEL94GYRexmnfMmehy27U30E2WTSFGo86SA4vjV&X-Amz-Signature=0ae07071c029231ef8cd01b392b0fbe1e67c5c12312a9fb8189ee7b771375b6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

