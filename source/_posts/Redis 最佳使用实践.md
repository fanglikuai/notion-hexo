---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUUZGBEV%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIB8c6npNETYnc6vS0syc1yzUnl8QdobfW0b9Kp2uofSlAiAyROLtxaUKe2EoOpId1FGsqeKigpN0xTEDGvB9ff5pOSqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMs9gyXkuFggnsJlUCKtwD7t66sFHhVDYSVRVAjbCQiQWVDkfT5AqAdm3G%2BgdSObsoUmaXgLxs7VyNUi%2FF9aSfBqJj3qezKd6mnoqKQ%2BwksKzTYcnjhonbzNSgTl82LtK4txI8zu0N3noD5tOMfZvQkacAExnKPVCHkWNOX3Iy1ZcB0MIRoAvoOCPvxEnUk%2FUg4VkFmVJkNGmyLEmlrvmOD9KlUzJdRdNPKCvoOde30fUpyVNFhqqwdfCy6ZunFobIxfRWzykJohnUDQtvRSHCc3LDpWurs6p8XD8gq%2BqInEaufOf%2BWnwdCuj9xFRCCehUvokYfaC%2F%2BKgjiD8h%2FEyxR4PfekI%2BoY6fYAaAggpdv2Jq5M2KRayruwjS8gpfN1tjZ2cjEavBpv4qfvq443lJ6UJ6Rh5uz5wlYwiHHeYjJxctX%2FajKPPDtuj4A4%2BBWR0Yu1HMmefys1Bw6F87PBOFXkOL%2BbJletMYWmFarcEo1Eytm8BX3xF7kTg7FRF2wm0RawUB7pILndzY%2F6CrHPEptP3wzfR%2F2Hbz%2BOEdXdvSfuNcBhDBn9lP%2BYHeE8PUQoYe4on3XoNb4EZT%2FQDGFavqED%2B0QV4mqSJ3WvMOghwNZtRjozD6nEfuNthG0b5G7xMYfZC5VO1Pv%2BIc7oMwltevxgY6pgF7M0j1RlJZHciRtvVQZH60p%2Fd2P9njIACBy29y30IZb%2BqEHfXNGw24FHgKGGEDIlBp1kP6S%2F8a4BROfShwN2e45UjftXfDQzXbhgjvbe93RUJcJ730eAQNRqrkR6dAKeM5nfF%2Bf1W3lb26laNx8hATmXXUIGm86mszksdwiQy5mIBLxBAFCFagRAK349tgx%2BsnN2khz2FxGYDAymAXdNCDx6Qqx8VT&X-Amz-Signature=23859134d69c6566d9d076c55f83b32f822f048e87bef16e4a44b82229905576&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

