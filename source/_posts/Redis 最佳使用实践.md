---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665M4D7AMF%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T060052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIE8GAcJPKlws0QS5GAmEJIDEhYAK0JUzwMAeh6ZTSnigAiEAkC6B9xhuPRLT9BEsLXmEuJit8e4aXhzM0CxEvvbmnr8qiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMurD4fXq4bqCO21iCrcAyT9i7BePllSorrBMc3F79dA9Ezdi9OdCnfSzzvckjcuRr9mLOAxXPcEe0rmLkuec3%2BTuyKNANlfhRDf7vFos2awHvzu5WGwzyjEroVpvxX2m3pMnC5eljiNNBjqFoumwiZjk1HForMoXAUJRnaW7LZejMADcvyOKJ6w3MPNDYanyUW%2BQ9Hz3ld10%2BA3hCqyZnChR0ThP522fDzpOrxrQ%2BIApFqfS8FPL41EhPGvmiOEWJhbigadFpyQ8ebHxZzYAc%2B7xbXdCb683SRnH6k69qGnPicD3EmA48vOqs1D%2BRpL2DYoru7%2FXGT8EzVsLhwpWdkmxSpB%2BaeHzUY2c77MYzohhhEU9lQ7C%2FbZqDU8h1DXTi01PWO2FRA0X%2F%2B7NCePQJwPNWjB3dIU1nVkRZyC1TVrkkd485af5yCW1Yr5STBN4X6c%2B2CFtg5yvFVPDZr389ZNb6Pag27LEehjxLACcbUBNzRyAadFjo7AU3mvCbYSOLsY8SkqWsjp%2BBYodcJy9GktcejZYRudK9fwOGSSFs3ahF8XTUwMz%2FcgbXl3r4ot5oSv%2ByUXad12diGUOiv2B3p3QmVtMPEXzRQp8UQKXyjQIwqGrouxaPY2xYuYN4x8YZaEL1WlDNb63a8cMKuvu8gGOqUBhLVEPQRld3SIBNwXItgvnzEgayxm1yMpbJ8nfyxYPW4%2FN78R%2Br4q%2B6jGFgvLKhjXMkCUeDU8%2FABWdSz%2BSdBkYaqMePhIysX%2F0gL8f4r9I8t%2BaSCPTC5Gl%2BRHNre0JBvixZFH8vWi0Hi1R7QIS0Fs0ch8SdP%2F1m0OOCotEOSt9%2F8%2F2MMrxq8MLaE1FGD%2BGG%2FY5ugfBuNx%2BsV1huk8jujzQ2cwd7nl&X-Amz-Signature=05dbb673c82d0f63008c9eff3a67b25d535f2442ec7e63e5e2dc03926d09ea48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

