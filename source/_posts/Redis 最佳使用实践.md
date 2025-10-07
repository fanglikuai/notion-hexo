---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOMH4S7B%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQCAO7NN%2B7FD4z0ud14Wrpn%2FA7od2EaYxmkJv1NJ9aCKigIhAPWJQceeSc%2Bs8eBUVhl7IQNaKJfnkRMPHnPgdeGg%2BC%2FfKogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxL7O0HNJ285oAq5aQq3AMuWZOHONKPdgp6Q%2Bb1%2BaYqOftjUxkQMg5K1is%2BbeXwIejas%2FGzid6dSzQlnTMTuPBXZw%2FJchK%2FF60hc%2BpSdPcXGp2lIDO8AAhe%2FtNKldhCWHeGx0I35pWEGuVuBtuIa4U0O%2BNKYD9WKplk8EnheTxyQe6YLcvJuT8162gTRPZmQT4kq6U%2FyvOuJMXsNdMU5A9KXGcVmnkD1bpbCNYRUMxUZ0hXcgjNO677OpFLmwYs8DweJphpt06wu3uP9Uwq4bX11CuAbATgNfwSqZUX6yeYKmurSDWxi%2BmGYrN4VnP%2BQ4EMtpoDlghMIFTyxzdozSFdNBus2N8JM%2BZ%2B%2Bh2q%2BizcSUAFBarrcCck4M0VCui34c%2FRwnmEVK2a%2FTQSTMNJe0kpouc4EEPDigGqc8%2BxC5b8Hz%2FmTHmBaYXlMpyL4VAYDvB%2BGs6gYPecxmWQnywg%2BnOTtXhPIXarrPT6h4%2F9EMt2F3%2BoDbrlOStVqo1YY9OfTXt8mOUoXMLzgfuGJ%2BJKCfcvXG0Bnygz%2FyoOgrXcA9BRildXFOw2g6PwkC0pNqkjGMsp35NpqXYF%2BPFjCL0weade%2FjK%2BzWUA3h0ve0Wyo%2Bdy6CsIk4XNzp%2FQRCKNa0T3zYOmopKk2cAgaQVkBjDNoJXHBjqkARJL2WaWuLPohZnNLMLr8AW09SN2W1%2F2M7prGw4nnA%2Fvt2aERPd7eBdKYg3LKz%2FqLcVtnPrvk0jrdHLnSY%2FvXEnu9WitZJXSeM4%2B%2Fgqoy%2FKd7JW9nXVvjiNMtHp5tM56mPRSltuIeubupNEyqhcAz7xJwR3CzOkGaJ5awwlmSMRSNt30cCOrsIJVzNVPYMnO%2FwCahk3hE4t4JEm3flgIAmctfeCq&X-Amz-Signature=84d517076cc45ed67704b3a089230723dadcd4b87b4043b2ff17d080a5fd7d08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

