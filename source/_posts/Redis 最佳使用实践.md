---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667X36XURC%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T050048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDDNQNRG5YKOzcc0fR%2BviyjgNeXi4btqudwQWb8nqV6iQIhANXi0X3Dyg%2Bquq2lAomC9LqRvU%2Fl2I0wFqajs9nnkvFWKogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyNB2zodrtkxzMGaNQq3AP3p%2FOKxSPb6ah%2BMbqlSbJKPntvTDjXa8vIRBSEWgtAHMmAYPau4pf3GIYzXWXwaKGpI2FRo8WTjjdzwLmK%2Fw66tMQa4G2VAjBC%2BdLQPTafDB3EBw30N6unS98MTpkhdtGPUcJBh3Np7v0hoecKHagJu%2BB10wVHkrmKovn2riE2SuN%2BbysJ67y4ksTdxmJ5p7Vtq7ZTY9NCg4h02SR6R9XD1fuGfBAm1jNAduzmgMuANR33Y%2BsW61Uds9NhBOr5egZml%2BJrArJTly7H%2F6EuIFqCWvNUa48ah%2BMBPQAPH9Mn0UgRIxLCE4Ry0Y%2B6NaBqe7h5ppLJsD5JxgbTqaqPPXsXrEAc6XeXT986u4v93zN%2Bg%2BaE%2FEnMN5GKujRS9S%2BcZkstujTb2GdQGcCY8aJw%2FzMs055rAy%2FbeifPfe%2B1EmEM3GaJ7FVLxmRvUzqkXvXquYxkyxVbaMqCs8cJPl%2FtiHvoVIZW7c2muySUpXFCD23vxTEhT7F%2B%2FRqN9jYt6enqccRy2arHBKmdAT2nTXqxvnXcGQgwL2viTqOGQxwwm2yJcQFzpIQVc8t%2FP3LO56wdh%2FrtjpQsqurs972cNbr6IMYEl%2B0CRgZ8iGu%2F5hw8%2FhVr3keNh29dkEblf6EXijDJwOrIBjqkAYxCrzwNDYklXGVejsF6b1SpRiWn30R4f7uOLushPZGcK2aFa%2BCHKVoo%2BRZJVqQWpLflHvKM7qu6KSx6zUFvo6HAssokpvd16jpqmGKVhLLvybSgD31BlsD1fls7%2FjVqYWmoXoBP2tTP3F6ba0xJu7SM6vzL%2Fy2uPQxXbk%2FGDDSbea%2Fi7Pk9oflXIlQs4E4CQ7vLko1OTOMLIfQ9V4w0TxR5yyEh&X-Amz-Signature=5faf497eb07bdf1c8aff53c0a52adfd3b95e0e5bfeaff8f6eba6c2962d3cd0cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

