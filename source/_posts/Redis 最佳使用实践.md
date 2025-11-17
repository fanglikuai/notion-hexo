---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667353DXDL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCVH2pmgatYQKA8Ts6EGMaY8XrvbSpz3NPIBTTaHt%2BLaAIhAMbyCKeKcU8vKLQKYZgUZazoNyeGEUK46Rp9Hg366ulUKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzpgE5osWrXXvwloPUq3AO6r9%2Bq1OCYYLT3TWxj5GSjxDfonCDHZGKb3hyvOE7hfcaFITD6vlqgBenC7BuvOQ0ziJ8RhI%2BdQb0z%2BBf%2FFUb8aUnPI%2Fd57jchzWG3kmMyD4%2B0pORmkXlZuS9ok3ymioYiRRc1luYZm7CAqfFtSEDL6nDdeSkejC%2B%2BPefGaUCBIsrE4%2BUZzzdgp4W3p4D9g33by4XRJ4QbUn6%2BplQaVGGIrMjcs07E218tvdM4eiVUT5Pd6sVOnv2xGSHf7fpM20ylrJUB%2FlGj%2Ft6Vvu7DQ6jnHy1ZI6ShAZ%2F%2FTB0qAzSozj5bl9Xn%2B6XSz%2B0eiXqcPc4q7H9TnVcgKzPAWKbtXWBbMUv5Zw2tjgf1g8sqFWS5%2BS%2B2frruLApBHLxsKYXkB4IXxD%2BMTDOov3nQibw1IyIjLUE4lwsz9BMwVJvqoKAKy4C%2BbyeluCL02zRCwf3xQphq2tlKpyNHoCKEFEPqI8n09GWT123xbKORpsePQqc4zrBFGvkI7Fkk%2BZdiA3e4ZOmMMkvuW7rWkIwv3CYCC1R66YdzXnYtivSs1m6khS7ulJuW4kPLLWLai87SGqKzXQEwfSNJAOnkam7jId1fzlINlxU8IPFYF7PI3gUXMBjUtd3GsYrlkugv9h9mwjDJnu7IBjqkAep1t%2FNeoNsETp6xTNWA5KMqvZaYmf5uAungh70VNmEON%2FLP14Xtkz0NzuvZKifX9sk6kFqUkoaki%2FcyB4YUjT86ki3akCI8QaUDdJ1fWN6Ax6uCRuJhq4b%2BJke0rpa6Nadl%2FWLTf%2FTnmlD40GYbXWMgCEx94M1sqV2Zokqg4sEpKYu6efJU%2FYxxnUeO9qNeW%2B98vroT0CQnktOF%2FUZnaXs9hW4F&X-Amz-Signature=210dabe8978c9ed4029fb3279013fdfdf8eeaf479bbfb4813a8dae6784b4c264&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

