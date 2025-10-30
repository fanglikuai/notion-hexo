---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DUPKNLD%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T040056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCGHnaD6tXpxPBctFEFA9H3EKpalcofdyKKoLEnZmCzCQIhAO%2F685uAuHHBzY8CWoiS8ao9mbGqr2Q%2BUolOKNjZOaoMKogECOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8RSiGpZQaAOcWLqkq3ANPp41uob71vCJZsV5dFW2Gfv%2FQ1bASCUmFw5hcJ5NAYFtQRusr12fnn1ZPvNm53TCJ%2B4bYc0JXVM7Ut3s6a4VhxL0VimGvZmE7pMw3wp80GZikFvtmjVQVC9riFrWDaVn6mG2g7mEOGWYj4x7uYPcBkA8irAJXRcu58DF6xxHaGmLtwLYV2XT%2BK0xzEi3Ej34B%2FhJ%2BZ4l%2FbeM09iT9KRFo1Gijr96A8rHLhpbOkAnOQQrZTDwmPmuZMrrcyfP2d02TxbV6IZus5Fb3V0sVCmqv6knkgG9sL93y%2FwCK9jlegHYWLjc%2Bz0rgmXkR%2B%2FU9QCUrWSlOmWsp13zyioNiYpc1qP7vd9qZO2wjVRowmSYtYpcBtIDNInUaWsnf%2Brq2x%2BcbLpB8enHzQUGHULQjJRwH1mTomxcSCYkXVgxXVfmVPXoeuGdG8Td3%2BPzci%2FGLG2s7hCzpL9BmfY7skEgD7aFjx4hHuBxgoLGJlfCp%2Fk2rWCk0rI52JDnJg60nmhAMDKqKz0%2BnbeCPrpFHfmdKiDxGiN2ZJH1HT9YXV5CME4GbIqbTAlS0%2FWuCFVli7VFvQnwwtpIQhtRd0lY09Jozi0%2BBYLgAlC5TystnLupKFv3Wj15S%2BcFKmXj0jmeEJDCms4vIBjqkAQZwK%2FggCNnBgCY7RFzEq3kqJM5%2BrhC45H0DE4h4dtY9sOi6%2BBmZW9c7o1ksuIU%2FRtth%2F2mu8nmGu5tzl6u%2FsJNlSSB5haowl75jPMq%2F%2BXhXhFCvKM2oSvH4qvnXcnOP3aRFeVD1hljrXLBVMJ1%2F5JufwrnYUyI%2Fz41yDQCsn2wq0snH1D7Y6MtaqQi6B5fgUJ%2Beqm%2FLGgEqh55RT2IaXoYIv9%2Bx&X-Amz-Signature=8fd215aa50082e6de56ced2980b992b7747fe142f4e70290cdee1f8c57c16c1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

