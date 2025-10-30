---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667J7NBSGB%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJIMEYCIQD73DIAzUVLwLcO66wsx%2BAHVqurBYGX%2FQX2EdqcjZWBSgIhAKoWEwf6khhqDLV9PfAWCwUnJejUm0jLy5yjhbkQNpMMKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzH4f3c9%2FoGf2e8LDcq3AMgtBw3ZUg9ZPrg6BZP6oXRpB8qesX0o%2B0oeVjqjsTt3bSFZBgFmOBME%2BCkO480ffhCXH3GJYXvrxpe3Qz20SAOdzl8X%2BcJuhpbPR%2FmOTPk2RxlGqiaZ%2BWxNGjemKIlj3ZIqOflVcd1zWNgJWxxH%2BEXD97gv%2FS2E6K%2FXj7vtIs02rjq6OS%2BByBbmQrvdMK5dRIteQ%2FXOoPj87ZMKWAJQ57Czw5DYJYoL51bBUHR1Mj3UAmHzH98vNvnCmCuX8LP9VFmuxipHNbcT6lg1FGvl6RoKYJ1vWgzJPHrjc06Q38XWTQhvnnIBdahARRQ9bD1HzN469AYRMDW0%2Bo5urxmAAu1YBgoQ0TTgYFIj0QXWH6eNo%2FW5jZHleXYjYMEKojiYA97WDASHjpR%2FnZ%2BFoptT2djX68pDUJLQMbUdp34bNJRsqFBTEJGL24CNhu1H5MYU5TE5oog8d14lRY2nyqxj5sJqkAVBZYbyxXNb5i%2FTql9BdB%2FnO9XBHmTXr0mImdPU6I%2FNO%2FBp9B8Jwh1KcbR1uePl1bZx7Vd4QmqedsSOVAUVcmo4P7ZNdEDpdW1MPvI4H%2FmeiGkQnKUCjiJz%2FMo562ajLe7la5y2WjEuMrOBvCspP1JddAO6chPnES9lTCxlovIBjqkAZ0sBlmjswc2cWfN0%2F3TXyaTdYxEVnfhaqU8PxrwTzdh89phkq0g90sFa6IugC0jNoHrH8plEmKVIyYNKCeqxZ2CHSPBbmHn28b7Wl8nLUabz2cmnpTP%2BGqjqdGmZdPE%2F0Jy4Wa3mnkR5nnXEVcYnGK4VH01ukh%2FqXFtzTxc3Ih1SxwwL1prHRKF93cEZQCAaXLwMMKJREje%2FwXFcavcfTxRL3ln&X-Amz-Signature=a50d1a886f4b18c45d0102fc4f2f1cdb8a06f486e8d5bd3310c8d63d687338ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

