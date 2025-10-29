---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPQCQDMV%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJIMEYCIQDYlcpR0Ao%2B6FqcWS5SO9nW7ewHGu%2B2nQVDO2yNmXiR6QIhALHA4617M7vyp49NT1VNXm5i%2F0xQADoAw3HtrLqYRO8bKogECNf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyDcIGWkC%2FfuTYXLdoq3APsgvYkrDJhLr0dXZsDgnZ3wGuqfGUWn%2FQHXM3Kx88tPhzTBu344cWlQUKEnF05MfBgJ7DaYd9TJtcrybXJ43OlkbrLbSHdetk1kVclWWrEnh%2B%2F4pkBx7ZaLpjEWv9ZO0fKZByzdtNEM1kEGcWv7kp%2BqPCKViffheGAbF2A8ythevq%2BY1NNhYy%2F4lgur%2BkKBpGcCga6wTxUN5VV0hSwj7E6p9yLjBjl9apPr3Ns5kjh68NPUpsmne1TRwj6qSTYoNgq9MPL25ukjhkUlSFvNlKK%2FirHyKQE5I3l8Li1ymEeBQwIEdpRqEr8fiWTQz4XrYMfy1hrT3IP%2BCVLDd%2F3j8fTUgeTNIq73jKFBygFmUmC0ZD%2FnMU4mnZMDyA6Mdem3K9lG8ZuEFCt6VQPXPzxQdgR0Ja9PVZVv95DxeiumKGg9rf8LG5eyij1%2B6SoZB7SCHcYYUnQ2s0k9QNjK9ptPGSA8CjLtBheY%2FupI%2Btwlvthu6M%2FUJUlQ1OuVccMDd5lQbUR%2B6nRw3AijXKxQju%2B20fA18GZlCvyJ65mpdptMIYkwzti7T80K01VaQw0LtjOcqlX%2FAiKH8B6n%2B5DLBzqxA8CIj8t7e4o3RRxJR%2FyJjUnwuwQrai6W1jq8BMCKDDVrYjIBjqkAYKK%2FCZF2HnhmzqRlodNxxr8uTmQRvNXHogGtEgzn5WtNAK8nu7deaE%2BufXnTjsno8FdeOj56sCS1ffBx0H9Oyg4zlfxRGmrjDagPVxhX%2BKJli4bM4iM8TqDMDNdMaHVjPdr%2BgIcPLR3pD6cfXFvKoC47PsVKYv7CsnGzcCBf3JS0s2ZEeya1IVxOHNKrtIk7v72XDC9IsUpSoT7Yow2ts4SPpAD&X-Amz-Signature=c29179b3d1bdf8b84336c784abbd02320a7f25ef656e25ad9ebeeb874f58ef68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

