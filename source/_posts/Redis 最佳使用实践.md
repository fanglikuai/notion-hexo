---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM56SZN6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCICCRQywGn33J3ZCF5SyvCgQPhD4BJoH3Nvm1tZ%2BVMe7tAiEA7TTyGUhWU%2FyZQzHNtJkqxoZaWKD%2Bm5O9Y8dmQw499wMqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCTHg8yRexoPLWHUxSrcAxrXKOpIMa5OMhgd2LcLoyNw%2FUJhUW4GAieI%2BBeg6L8FHf61T85OGJnMAlnivzXbdWVTTPZ9iTaLI9ZjDGgg9SJxfPDBlpKD29P%2BcLvgcicJfEwJbopAgEFFSH4bsn635qGWlPcWJXNXQQ8I7EiatEAZxEUWB4%2FHFoCNbAYrWZJpVm3LLFjPxHWw2RvcUwmktIe3CBZlFTbrMnEXGRgAuDDIVZWgstoq%2BmNKPsHDtyTtaoMVTt0PS0x8cutTWn%2Bnld2kEaiS0fUesIaKI6Sb%2Fn1rfntbA0wXevHQODGJF56Ho8ugtR30OgSSPQ7fcnGHiULw%2FYaRz6qg7ae7cSXf4zwM58B%2FKwXh%2FWt5SDow0LpZ4zBTi9KcvrruNyU%2BaRev%2Fzc%2FTNq20fOqLN7xxOosN4sknDFYqxAbbNPUY%2FHqLCLdiCA2i1a50Kit52kiIxWMthwlAf4ezLNLpwqqy3q0i9zTYF%2FujnwaxpWVfworg3oxwJaJSSxZQd7AnwI%2FW6A4vWQhv2WJhl6g8n0QyDucNSzcJ5gQuomM37n%2FiCga%2Fw1qWdqiBZVsYtu9J8ezygklIujUI1PBuD0chsvdFOgac8gNvs4pcLidbS4EMBE0iMPAqDJ13oL%2FUeqA2Y0fMP%2Fbp8YGOqUBZnwxdBNmckSE3dazoH2r6Kkx%2BGfpWX83PnFTxLuwXdCWkwjXbwblXaH%2B4ALtZCSJ6i0Nlpm%2BpM5BATsAfeJfJICVmlNmQeZJw5XWOBCBwvqQeAjYf2n1RXzEE9YimSXmLaMuzbut5CT2bvpBzOcmG6GGL3F5TmaljtfqkPhkKeifulycE2os7DcL8WBOymIUFppCHz9uhxtBEDn52kSJl1oY2WYP&X-Amz-Signature=1ece1af4f0c547d74b4d916f2eb4a3b6a7cfa8057c7527d6443d7902cfc0893e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

