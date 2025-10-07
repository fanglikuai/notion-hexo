---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGKJEVGH%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T110037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQCUqIgPrJvcpSG8SXOn9U34giEPGHD8H%2BplKw8H4ACjkwIhAP1FFa3B4CfLSfWxvytChbtNAi%2Fok%2Bexe%2Fz6hOCStwnzKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwUZUYa1YUo1YDyfykq3AOjHmS3w22cr%2FanbLAWNSqqnRYZNwAfx41zC7wDADayNyU35bKkYdGOcD13PW%2BlAiVNKmwTuh6jdm%2FRcbVoVDi%2FJcb0UDn9gEUviC05zxjB8hd02Zi6wgJBSgE3wITNdRkkV%2B4QJwf%2BZEgGz7i%2ByZhFfBwWdUGtp2NydAUxSa8c4XCKsjK8u54AOEl83k58xTWEAW3oTpYuU4vA47Z89COJ3GP2hG8utRXKS%2BPLRed%2F59DBioGlQNhrmWzLzCN%2BUw6igX3%2F9tJg7pmj%2BYrY2UQCH8%2FfNuQctPQs0rnEv%2ByD3ko6LdnGrcPvpyAppA8S%2FgmEO6Pj1Wra0YLs0vDqnuEpMj5ClcMrT2Rq5bwxF8fKzHpvhrno1Z8NU40FopnMKAuE3CHgJbGIBFFZUD%2BGofeELn9QXZxfpoQsqBh50tK4kiiM5F8dqhRi0nnSS6ADG0xV76lIfYpplQR0aWTzjjm%2FkTX6HfWDPPJgPlNkhYgmwUPzJhbI%2FucLwtnyp1OJCMqCEfz7%2BZMNhmkrxMDR7gRYeXlfV5cQMWbo%2BfrxFr1xh9zf8FBj00jhf%2BqfHzwRUShp8vF1WbUbQS%2BIfp0hxfABeGMevXYLMJ39p%2Bmf2Rpwdl7f68mvDm8M2a%2B05TCW3ZPHBjqkAYc1C7gUsfnO%2FzvCuPbk7UTz5PpS4TTNXgobfaUSOKAVUVj9LppK%2FkVdTAsnJ92JVf4pF6oWlNdNam2ViLfYTSNHoRL2IpEghk6o1Qils3%2B035COa5fL5eCBM3yYJKq08Q96JScSmanmt3iDifSXuJTYi7lF%2FQwaRpcHIz4fJlitjqBKUAXqFkM%2FbGo52hRMWA3%2FvA3PvLF2sIYjOvCCGNQ1Iti7&X-Amz-Signature=68e722efbc4e301b60f709651fb48d7292455168c149f708422411891b6912f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

