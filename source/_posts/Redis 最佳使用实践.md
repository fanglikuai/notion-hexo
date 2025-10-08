---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3YA7BDQ%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJIMEYCIQDPK9BC1e5Oh4LjRiIUZNHDv6HpAnw3yQleor5%2BCvj3JQIhAIo1WreiJbmHOvs4o%2FoKHd7TUFzPr21%2B2RoUn7N2TtpHKogECMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzeZODFcmMi61jfJeoq3APasbEqmP8GmUkWF%2F%2BxkJt6SvWStWp2YzSHXH596iIJIrTBjVjnVFKGzK2xiiHjjGGb8%2BLbAgDxZAWqFxIqWtDXRMiV0LfdbcqZcH2u4j2v4SRvvWXzKuu3P25ar%2F736xd3bmpJs%2F1xz9jbPhgJdbmP8F%2FqL9xjtFHR79JB51Vv3MQGVcVVEetiywTQVPmB1WIEwUbynzygK8K6ITuBV4X5ddNAIZpi2e7VJAbuL4irghd5DNxklf5DTdo3TVr52wW988fqM539uiUcAbmAN1DbKDnl%2F%2F76PYzaoQ7TrhbzWX5d%2BSRiFMzJ6yi8WPf9x5B2Z1lTVlCOINhXj51WX1PKud4zhzcT5%2BhkkiH9FkagCBNDbNuflU7ZmWmIrF8FVf2ED3FaXMSzOR5jnSueoe333GYMQVdTFwjRp%2FvwmAbuFSl%2FpAsD%2F9X8Dbzy141ROufJOvC9m0cERJJokCS%2BiuyfandIiZAK5aIEcEi3qVcRLfgpqGOi4IJL5cBKRYDXHtE6Yf7hnQuuAWQTr5wLFpJCRp4gVt6KtqiMrJA7SIuouRhNKQ5QB83pXEdRxtcNH2ti8VWKEjTNvxDJlte07K%2Bb%2FEJVK0izhaJURcE%2BcfVsZoYApfv8seOKmrUzOTDOnJrHBjqkAad37dy894Ixpc4zihwkT5V6owvDreaGkeHI3RCzW%2Bnphd%2FtIPyCyQHpOsGr0Pa0eFB1niIQdYLtk4r9XLqakqCLRILgjXa28gAyheQ35yrbLoWBU1yFcT1%2FWPNDKDYNgQR0zlBNlqeC8z5qRXoIU54kyyFhJGLGUFcS1nKMzQR9tgGludafwN%2BuPC2SnTbfesSvNVpu0UEX6xN5XIt3hiTpbz15&X-Amz-Signature=9c5fa60c2c16c6793c88497214047d78e6742197006f11c5e71e8ff1bf6bd6c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

