---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWKVEDRS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIEa1frbP9qRVmeMYYfAicxaS84W8sEPkrGumLz7NDKy8AiEAz%2FTj7xbPeaFLAeklE8QLLYLldSFp%2BTGfi33%2BYz81tjsq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDAqPbqA21fc1AL90iSrcA85kr3J4P5ncC4AH%2BBMHeQ2qRkktgKY9onmWt5O150tV7u6rpI4QhSOTw0W1YIwllaQesRB4xjB7lJGpFs988v37gthlmkT98Yrus4%2B0sCcs1WNiLyr7U27GQW9h%2Bz9%2FZjQwnntkUhRxMsNMHHeNAUEVfyD85IzsrQUGXZS3aE9vWYNhDNCGL5i8ry2bq54dY8GiFednmM1lPaAGFVDue2w9KQqYMLjC8vFlOICbtRbIvbBD5vHIDA7ZLCNyJ7y01sDBTr6%2B82CQpGN9BClQcMpEnXtzB5buWHyS8KG4U6LxE1bYiqK7%2F98H5wsphVyKgHWHog%2BOMWTaPbCyf61V72lTFvhvjVBAgF%2BYa5MoFUS5%2BE5fbZUvNGOnlDhqJlLRHaKGTGay4EOdRXMGaoAdpv9NLVuHY9DEAFrtc9miPfMp51jR8lLed2MlzwrMYzTFPOHKqsFsfFuCdGgV61Y4cD6MLs3vjWnv2kJKGJqV9Kt9PrMIBe2DwW7Bh2xhVyDScIPYwrNQ7i%2FcQb1thtt3EriorjGqHto6oXa83xhEAVyCg8B%2Fk6ic9FvXNuzNDZZ6X6WKlPKi8Ene22yPsf5YSmnPp9JIrNgkUy79FBLbIZNihtoQYkDYiInVltgqMI2c9MYGOqUBSB4EGXDWDPmZBvkMjKqoyzF4CByQr6aUz2aeqsPCI4S5MM01mQ%2BhYC2XM8VUBnBsL6hKGwdRCHy%2FczR%2BS%2Fk6ZpzHDo9pldv0y2AVOalBBkNN4QRBIdQDHLdrbG0L4ypeioJNYc%2BDMe6lNOXYbobFOe79UL2Edzi%2Bmikk2JhBSQ30VQoEKuY9jCTxMMZVJDUTkIqtKp%2BFzzT%2BntRx8msJyQcJ2ENm&X-Amz-Signature=ec748b3036ee66a7dcab54ffbed0a4e6dce4632f43a619a70dc58fb7bc46c0e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

