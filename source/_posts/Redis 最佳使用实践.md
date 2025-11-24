---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDCOJYKV%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiNWV1Z89%2Bt0j2t%2FRkVq9cLZP3LHJG44pu%2F2R0YtRpJwIhAKNV6L7BezEtvu7jcuAFJMQn3q22XeZvGiISS0BV0r6jKv8DCE4QABoMNjM3NDIzMTgzODA1Igxh4CeqCdDfEWMMh64q3AOYbGNF2Cb3G%2F2qc6KdHunPJ0tUnzYLLMbn5nlokYoK%2FvjpFoVEeo2x%2F5XwOs2iD9JEnqZ9kltge7%2BGr0dxVea1RfUfMA1ORsCdWTaBcyfYy%2FfBPqTqlVTYu%2BJfkjCbN%2FuBuluCKVkh1aRkOc8xWvpRizNRSbgmYnhOZLf1zuz18KXT2rY9Yss6fKxBhmz%2FWiJlT3IKySEVS6Qemi3ZksKHJqtILAxYvgMemZnQ1VNjBLNoaq9QDyyACri%2BaIX67A8YEWhrdMY0L2ZI8RZ8KJQrngx0oK2ip2W6rCiTPRNzlPmZ6Tfx4EkDj29QavngI1TzqK0dFg672Tgn1d2TMWdu7EMTDFXxwJulMzTwXCyi5W9TbuJQfKV15rIhxD7Tj9OrmHnRED0yKhvBlSB7Ot5Fe2xPzjq8EFWn1hHlN6Kdi35iypHzwyeEQMO1Aja%2BtvlTXl6FF4z9zOLQhO47LX0Bg6UguGnlQaUd%2BeeoSpPjrxPiiAapkqYzAgE%2FGjAZH23KlihL3VPbNrriJR0qb3wZHfOLxfWulxBg5BuPcAEt2%2FjhfmrMQ%2FXRjDYde%2FPG%2FhVI4y8p%2BrXBxzzA3Bkbj3SvRMPUE9dXQLV5SnBLNU49qoZvGmICvdrPKAD2vDDyyY%2FJBjqkAanJEFQVmqbuQv3GVXFvhDuP5llxutXX0SiRnDcaCx0XEGRKDrROuMocD9hMNMB6BYp8G%2BD4fOk%2BU95kw1eEjfaOjuREXm0AHrQ0Rnt1kJwsUVBqYrdka25QbTzGd2dwe8%2FOJ7exQgl%2BmbttGcMUy64WUg6dtFmd3tKAqm4sStt2DPbnoTPHH%2BteSYfjGeCt7FZeapmaZ2aObQc41V3kqK4rvm1d&X-Amz-Signature=851cf22b40233b1a60cec3765207709e0e56e9e04b036b7426cdcf186f72de71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

