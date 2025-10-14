---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3ZKL7US%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICu9yVcnD7kQMwAWrXoZDPd6aFZykNzjCyy4MtvEv70PAiA%2B7PulOPM2tSj7sEG7blkpMS5EIoRSEE42FaSKVSzsDyr%2FAwhTEAAaDDYzNzQyMzE4MzgwNSIMD0xwF%2BxxW1b2veqAKtwDu6T8DcmArpVhrIbHkHPR%2BzxG0Jg%2FM8em%2BcnMsZLmTk8M6WUDduGvEkFmg1yBe6azmwzI1GSPMpZXskii5USUL2B2SdRwFiNjrQ9PqyYup5OFgBpJScXqU4Wp2xRcVk8vVOvsaP5g2eW2MN3vmTwtnq%2B%2FhdISOK3GJTfTwjkRQzHpLempy9zXa34jxgYlcSARCdgl4sWdlJjRi7jl9ua2BnBk41nZKyXyk4KOKMmZrMB0CSahY6DUgijkEoBL1Hm9fYkiLktY5Bp2LcyrcWqTlMpyBQW5hzaqFcdu5JpJ4qmhUK8VzSb8l5jNZPyQzKHQlPqYA4vJK40mS1iAEeX8Cc1XO9Rtw%2BwkSNuf1aS7DvtuUrh8NilECgOn1CO68gz4dVUhcfxfDNONVLHBG9ZUXKQTV27zLqos2SZL%2FQvnZrjgfQu9%2FYeXJ5H5nkSuuwZv%2F7n8Egd9x1ArBNkjnNV1u3%2F5rTmb260z427TOwjWYvx%2F%2BW%2BL65rzhMdlVd5W4482ttzpIdqDNBgitQolQhzbpT8RZdA2LXzVQs7GDsmTzclcPnq7xE%2F%2BtQDgPYs%2BQwsBU5YWMOT9T9a9FU09cldtBfq0EFCc4P%2BtxcCamWTsYNScWfjs9DKGC0iIa1Ewm962xwY6pgEGJGAJf5U4dxGaaducOJICH1gNsuU6tM%2BE61B542q5Yl8ocw8%2FTQjw3D5QX73fBguFPZfkjY4DJGM2HMyGQrZ8%2B%2BNi3iafYT6fo6hHntAifjEE8AXvpoJ7dyhNd9FyeYzbj%2FBj3MPFXYhHHvCxTYDIYiBIAbD0mT%2ByEs9R1qxS5Jg5CQ5EFJ7wbrAdeoPqXybO3vKWhwIdTOyNG51%2FepkJrYRztPwQ&X-Amz-Signature=377bdc60280c187035bf5ef9f7a0e1a11dbadc6f374313dd8c0692f18835ae72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

