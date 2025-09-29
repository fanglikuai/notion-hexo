---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZBOUAKS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQDLIjaspxtruHv%2FNkGASlFoWhtU5Lv0xK8LGQHlLBmHuwIhAI0gWTKowHF9gtNREmj4pDstR1ejzs4zPlffqvsZSe4rKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxTlcyhP9TzC0NgCkUq3ANeF1HC6pOfz%2Bs0ftTXz7sE%2B8Liyt%2Fu4BuH7qHBxzO9RpSkgu4rxDzhWGHfJZ14eWnPh5fXXnNT39f2%2FdVYzRpZVuWJSMeyW2EEg2PwtP6XPBxxH10xNA4ts%2BF%2F9KCMJmpq6ZEy2e6hn7XAIr5aDALObET6acKKdxhl4ujv6PqCTQvvrna3RpKZAuOvMh%2F%2FOUMJfbmhjhLiUbmo6iDptUx2F6AeH1uQ5eSc91sL2bL1ThunK2qk%2FoWPNWPjUq%2Bxk9%2B0%2BDPoAO45JR39DEOB0X%2BmX1v9IhcdLOGGX3v2%2FOF8K5JVCnr2BiBS00u3V4TfHWc%2BujdNM7bbYAzROzJzrgGHxE4sE2OULHDhP7lIJNRFbIrbvz5RKhMLpvZwdA%2Fwv8%2FvQO%2Bavl5s0B96no7TyPL9WTMvXok6zTvFeB1u8POmTElZm23X6tgemcwXswGQ5%2BeJrpMEmVj9qKubWsnqi28r3DYh7BkBHZHM%2F2rk7Hma8jTRaY%2Ftg8LeDbjZQMXDNoxduGw7QeDgm2LdbB9lVnLOa9n31xu1XvIzOlW6lToQDGjd0vpqv6Qp8CmhLGVxFJm7edxMl%2FZbUvpbtfa9hPeQw2mkcPEaYLRPxSCefhZo7QwYdBMcxRJB%2BRMHBDD26evGBjqkAVR1Ib3sFV1H4Mmgmh4hL046agO6gUMb2%2Frnuvxy%2FIsb%2FDQWqo9lky7JBpoKTkOB9ZogBQr%2FDdvWjLWA3J2wLjokV%2BGdiVU3%2FWx4wQPGIYTmWHJ0Vb0SrSKKM80WXCn7ELRhrwLgdW8%2Bxigj9Hk%2Bt2nyxKpB0BRe1bUvv5pX2fpz3ws0BA8nAewH8%2By3pB%2BPEdX0lc72vpqxJOmkcG2Vvxf%2BJP06&X-Amz-Signature=292407bc327442503da2aa557e1e03411675f8c20263ca853ce9ffd66d9f127e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

