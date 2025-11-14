---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXX4BKTM%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T090038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBWFcPQ5LOVfTuUlKrfBeBi2oisKVxlrLIVHGavxEsMKAiB34AE0MooN27mK3JHs1KgpkPeOvvRnakHPd5yPc79BOSr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMfmBrK4voykZl3%2FgLKtwDB%2BXJEwgHSLiv1yv1x09G%2B6ZoILwz6SUs9uxfzPB2Ulc8NmlX6K4GUTAfArXk7ObV2PsEw4gyHWjcGIvTsX43ZD50wIm4dE4duWMtx4V%2FzhrLUR1KxEGgE5oM9X2CrNIHqKBLKI0EoBeas1po5OgZS8zWhZ%2B5ght%2B%2Fi1HKTOvhKg0OTsxO%2BWacv885Lw%2B%2B6cUU3YPo%2FLp717%2B2JRQ41czvVrvWSWQlQRKNsNBUEBWooNOl22UdOVW3RB5jnBGaTUNW0LAUf6i2IwoSz0lMQRXWS%2BSRK4ZwTJznkpd8iO0l2eYdHJdtCWWAQRYdm0AxlzFjrhBlHhDVYHEvCjwj8D%2FhOqYkX5YxUd4qyF%2FbayS%2BYtqmAK99ptoepqifOcObbc1x%2FRCvzGOGMT%2F2unKiFd%2F639rXXTWH0aDosuPPEOxZHmaqH%2Bjscs3c8OJaMAMHexihIiipPK0O9JFDOsuCLLFzBSuLtOUYaaM%2BQX6Hey%2FBIym5jxg6aSw8wW6eyARP%2BgEP7Twk6GvE2EItTmq1h7tN82IUvVIozGhybez9HgrwfflBqRewPmHLyx7hdTDkTB4U4UV7P0pd8lxELXYCZGVmZt14WMly5T5q91ZPcrjCgkZ1DfaX2hn7LAkNagw58HbyAY6pgFfvMMjkudi5qS6mOwx%2F93XSmLj%2B%2FuQxReA6de51o%2B9Mo0K3YvGU7EEn4u1f9fcco2mJGPEK04xAqJGy6DeP12lywAVW7yzNW440ljBKBrf%2F9tSHpsPwrnAqvVCv8ZaPsEag7%2FjG0fVE%2FlT4iR3j5w9ZugRqerGeXp9TmmftxN0TFE0yZZqNJl9MRBLJ7bHwrswrY8y4i3EujvvJwwO89Ptf3D7nOiV&X-Amz-Signature=e25672a62d36bddba8ab1d417581678246d5589c95baef573b73dfa811d1e333&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

