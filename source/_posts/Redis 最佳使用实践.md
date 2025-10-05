---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIGX7ALK%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD8eH8SGZiXcwy%2FM0h4wyJgSBwPmi3IqcAZUenYW3EGdgIhAJg03b1%2BEuSkJl7kpMbXyAn3FKkMGRpwnvCjkh9Io%2Fl%2BKv8DCG4QABoMNjM3NDIzMTgzODA1IgwfvJvwQ0TRT%2FHmgFQq3AOCjbCQjC8hhjCazYqL4%2B1bIM3Cua%2FIx8wBn66GXnSbNRCqizSj6KASQtfpA0UodKlDp99kMY8tHcH44rVAA6aQcG17diVRep0d6Ev5qzD5Xrs8pGyjaX5jwDT2HNVus0w7MVMMmX68n8QsfHguPFid9RWqGDp9aYqYLDyXVtdYtwXWO85RqAISK%2BVVFKZLFM1jvT5D3TukLUMBXbQOvnWnWRnHzUfaJELLtV7wAriRhXjKuPA8Uy4P7vSRigHjHL8%2BM7H%2F9%2Bdg6v3QFtXcxjb1lfG41VfXM79lE1sugGnq5Rv4FlSa8jBVziujsviIaXFyPGOVVDXURny8ENGFMcTNkoyaTKDSB6QK2nTMo3DneVKEy9gLjs0cAwBNZ6Ek16we6fZI2JpIr1lz1VyZWwodvV5ZUXorsG9X%2FtqaMyX2eHYVt6E0%2B1RZwbK9CRYwmj8JJpvnUgxp4YhqetkZgZ4CZG76IU2xh8HtsqY598mJIi6KiupoN3dtu58dvrmzogtfTxoLOgpuQBItbxNd9A0lHcV2PBVFlEDhO9cS4Du26Dnzwe9%2FcV7dDYZF0GFwjwLOdvdsr7QeJVTHxE%2FOa7eM6GffVKBtCM2dNsPJXjto0dvzRoux%2FzH4EIA2tDDg%2FIfHBjqkAa8NR7YPFxPG6tDuUhKXYhZY%2FRnd%2B%2FJ4ag2fwht2KfQzAa5Cz6QQrrzC%2FxZ0vsE4gWKw2L0homItlmI4T0a1BtoW9r0lHIH7Ic98Nuz%2B8gjrKjHAA9LGCXlkhCJ4Z1e7n8L0ecZwTW8gqGQJ3nbBa%2FQkAOwK90yfoSf21bHRv6dBbAcp3UvyCNiDY9YcUxRC5Vo5ypCENVd1rnsLso3ue%2BxtCDla&X-Amz-Signature=a82a0302a7c559834342c200eae74b6c309ba086786c73bd7fb3a0522720dcf2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

