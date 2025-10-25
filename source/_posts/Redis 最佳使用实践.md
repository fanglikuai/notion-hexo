---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UO7XC7V%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXlBVyM2gxTD6xtDnQbcre6u1eGuU5BwyaOBE7RilXrAiB5uUL%2BnNexNMMD25bsOvdfD9gexphrJ7lSJuAzzyOBhyr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMrxIgqnRwF0GH3EeoKtwDyAvV0gM1GFYbwUN17dO66z7Kur96kGDZHVdeUVGn2cypmb2zjokhwJLRlMcnvM9PD2zaa%2FLd0ovMBu1EroUdH9Iqs4SL%2BqqqnUZvJ7Hzl%2BAkMujQFME1zIPk7yKfUAJcF9cKMdUrguUVspoKBBJbpf%2FKB46Om0%2Bc0jFaIFKRd%2BYyq0ozXMm3s5GEyEZaR2u2MeD0YUL5eU2pmQMg69qNpaxfx8tWDPYRBghGLJfzhWLSJKbD7t%2BXKNbkyz1igr7ILN6nuzoQlkrhp%2Fb3u4TGKYJUxYBrrZC6QnTyfhACEPaf9p%2BCMDRumw7zcJFaAiwvZo2mjx1fzjxrkaNSu5U1ocqcuIySIxyfdB3BjJ4WI47miLOANzrBIXFULP3J2v%2BsoxdecEhkHGIDA8RqekcHd7f0sYC9d%2Bsx%2FHmwNt%2FJVEhFJhbGmgQhUr9cmZ3diGuHeJ85Y%2FDcMqH8gCMY0w5XBzkYeB0%2BzzBGGrve%2BDEkprLJq1ATaxbgw5OCeVAIPmhQnF6fg0NlQjjVFOoyfIiEdGbdg4o%2BY3efkDCqgqhj7xFb7tI2MjjBq%2BdE0mrV%2BnZ%2F4o2Ofpz5%2FNOzPUWG6G4oic50tqzj3hpyMqIK6SVwd2nB5F%2FAFOzBTBsrBdYwxdfyxwY6pgHLemj5Zd4t6pqsCeqoMJiEEk7wfBdwEDcEzzUDWFHfySByXMss%2Frl%2Fl55NAJDT%2Bj6wPqek0HXcfXbR42ct03lmBiUYn%2FbBKVRgrbhWTJmB3KSaKgqajqfkWHSg5MfHdaO8S1u%2F69%2BbXkn0OZosz7rKIDv64DiYKVHNOJyPeZfjQqYKAFHx5lkMG2A2K9aGDRej4zYq1WcAeR9VaYHq1fLcXpKLZsR1&X-Amz-Signature=7a9ae3df3be97c6f5afae8b81ca5c930eeee1ce40dcbf2bdb0d8781daf3901e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

