---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WD5GFKYQ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIApZxBeL%2BCmAhPSaSL2DdCGNUWTey7HWK32LuJapa2b9AiBjbrlspTzqNZT7Xyo0NfvnESPtZ68HfH%2BQBpzQsTu5EyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0IMDIEu%2FN2GjFvfRKtwDK9OY24344QwKbVpxlkfLc0VoRcqiBbOJ74jeaPcYqfVGolcep89NiseVb%2F7rrr44Fp%2FQi3KG2LBrTlA8u9dknDzdfdyfTxQuZglZpP002lozKjX7QqLVSUnqhht9MmJP%2Bmhz4M5YgPtQXzE%2FmxVuaoXr%2FyUj5PzeyCz%2BkJWpvk5s0p3M5bx9sr0PAISHkfjQmBFgvUSB3HzV1P62dhw9uvmMptDpyRVrd0d%2F%2Bved5yxIFGLL2eVlqif9dCHhtq0mJEfBX2bbQtIAsoVSc2qLqwDJuN8Jq63RN%2BcwlYvONSrrX70aJl7Wc9xqgnWncDLOviDwwRY2A%2Fsr%2BCvKAtpAmV5ik%2B9F9OdFwnQivXOuPZDAokhRvU0%2F%2B8TujDYPVcxoO9LXsA%2BuTTO3%2FKT0wQfLdNmpHqsowkzb4hlGVk%2Fq7nKsHipvmKTverUJBwViRf8iG9c6ajTWUeOA1NZ%2BPOozeiVzZKJd1heWKuLeGFf45G4ABhWm8pNHsZycuLHimmlm%2FNR%2F%2BitLaImWxavP5q1s02PCEneI2xgtJg3usaQD%2BCQfXov5nPGR40tXrko%2F%2FAV9BNnJz3MwyycfUK19i%2BmvbwH%2Fl4Cmd%2F%2F7XeyoTq65Sfjfj6D9ky8BoK6NYVMw2qvnxgY6pgGeY9MZowIVvSxgZ7HmDLnGETXvgZF3GwwDrGZWNNMkEbeW1xefL9ueMQXkkZHkJtg0CJoS8qKLzqihH%2BOFr6I1tEEUBM65uSyk1eYvqwmFneRBdnpTVQfY2hce2ynewGCCCUEtzSaRhHz23nGn8rhBnRHZHgmz8a0%2BeyNsk0WD72VGmVDrtLy%2Blj8JY5WpwYrEqth3LCTpWVLO2eA%2FkY5unmAiUog5&X-Amz-Signature=109123298eafb631f720a1d06a9bfa282ec7da28f024595761132707cb122216&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

