---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3SANYLK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJGMEQCIGegAY%2FJ4ODMC490UlFnzey1Ex3qzBTGn5P44Gb3uCqqAiB17W1wDBgkCqxoTdWkQPNX1jAyCThm2w5w1BAGaxsJCSr%2FAwg0EAAaDDYzNzQyMzE4MzgwNSIMiYuVjbTgv04kBTXJKtwD3GYvV5GsuDbUg6shKA6aEAWOgzJqAMtQXlc5U7bhM0Az530EtCGXQ5ZGSL5Y0MRJW9OTuaV%2BoacKhNDnmcAUnYhCqTBXZgmFrSskKsCfDNAZM9XIJS8VXt6fijaTPr1pMWPrfcNYFQBAg7V%2BTqTk%2F7s%2Bch1QBH0%2BI8ygt9RxFG0aXUUKYMLfFk3BpI%2Fg7FnPm2liS3r0pS1zSE4v7Y6k2IStgvsSlHjx5lOIL5s5i8u8KXBMfr0%2Be%2FhT4h5izmQYwFGB9ObVEOjmSL7H8gdxMSAjJsVFT%2FuzeHgL3JsHALTdGU3xJAzgwnz9P0IGM6QXcNO0wk7zqh%2FxvgSEjq1VCxu4ROJjLxQ68qtjm1TAEufJYfIByuvJPLjbDMA4DsAVo0FzPWyTtJ%2BbrKjE%2FsZP8jEvYlDhaJnnDIQQOECqxCywQOZzZc5vnScoeDlamc80gYwCuRMbwCZH4VlQRs3WTN3JO26Dr5rQLlm7nVjUc5%2B%2BwEq%2BJzFO2jM7ZRvBS5MEtyhC9jfUBwmm8sPc1b2%2FKmSIHCEQOZS90fJ3tP5Wfo246rjeP8XD64XiSqrqManeC7c58zXtMEa2CjoHAqzgrNgcU4gBRMtdHrok%2BdxjrrfClQSLheOnJdMfkTMwudDRyAY6pgGz1gWpKHsq39ysTCC2yE41iSejrNrJt4MHOh0DrCM69WQqMU6wQbHKXlQT3%2FglzFmljvUUBp21vadHv2Ko5rVdlEwK9uTEHl5P15phsRPWDDAJrL53kH1H9TOsix2bWNKUOS1fULoEzjWYyV4zCL%2F8oDAUhDcgs%2F1I6jP66Ot7M5GJeMULrRXimetZ7hOLTqjZbN%2F9oehcxtNLmmV%2BYYfKXig4l05F&X-Amz-Signature=1c28ab560433d79f5e6220b6fe01df0e37defee85e140e770b123639aa97c2af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

