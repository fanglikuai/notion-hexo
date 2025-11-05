---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665G7HWCV6%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHWs8f1HYi1kewMuTAcWJuyfGLJi7woLge00PqtUaRIaAiEAxVvvrRqStxc18UJmdGwGF3wyPS5vgFio2FH9AF8zNE4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPSrDLotStp7SeV0eSrcA%2FuFwLzOCfU76jyEGIXoWQF5cYq%2FfhqxVfnbmFNsLnztf69Bu14n%2FKSyphQ0DzSax%2Bm4JcbpteVMZDdyDwlK3Adp%2FYZbFqa2VfNK3U49xf4ZP4h%2FX9Rj6qFE8dk4ri046Lt1ZZwdwUPxmWTvqgHyfuRTIFXotdxRdydzsJ4Q%2B3hCztlTU%2FaMinuB8c6qDgYwL879A9bOk5RA%2FEf69jZYSx%2Bygf7q0wt8VuZ19fyTI9MbaD9v%2Bg%2Fg%2B4B%2BqwOlOad7Q0Q65dMlziwAvVhTKhVdcit9IRDkwvrLvXyentwBakOGFG7McFKdt%2BLg2JME5cMw4hY7WoviuQw9oxOcheRZ7yDm6eKRHgiqpHmd9EC43UHL%2FWe%2FHIkc1GJGk7UHwXXwHNjSYArwB1WRzjhCKDXs0%2FJmEQ9BprbXNQcFcdpInPqX7qK6XBkGgNq9WdM8U0MlXwuJCVHTU3nh6VKhzEZwQ9bq9MMZlsMujnBAKps6ifNdZk6vJAtWvG%2Fi%2BpwIxZB5tIDPXBqOZZwjSelV1OkOggn3%2FeXx27B0t%2FIMDl%2FlW%2FexQzJIPTC4jxOqzTYGT7PX80bW15%2BNCJ1CBOCM1ABn%2BI82PlRyduvIQmsw%2BfW2U7owPCGLvnPmfYcoamsQMIzqrsgGOqUBzBe1kedSCI60ss1GvyCrYWDCHfWK0ODJEJROGQqLXwO8Bf0%2F%2BNOYd5MZZe3tZvT8YHgVO7kIWsVjNaf2nLlPOWjEgDiyEGMqKDUlUZVvwH7zRfO2mLl9fSOAEWg%2F71MbKMW69z3vCdvAMZils0UHDdQGaNnoOY6h1gnvqyoabhlOI76X3Xvkpn3Sh2pzRP94LYD%2FaIknzzZLgDQrKYRLnEMPf16P&X-Amz-Signature=28071caeffb2423d7e4eb59b12679983de500a6ad9160cee3d57b49fb7a546ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

