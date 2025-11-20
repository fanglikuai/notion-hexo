---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNQB3OHD%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJIMEYCIQCBvwA82F90mxb7%2FyJc9xMPqfARID44gKaTVU3NXglnjgIhAKAgQcocSp7qzS9EA1hyuKcMTKgldeBAvidanmXARSTtKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwyuy73zWn9uampfncq3AM4MT940c%2FsmOz9mnh%2FSI2%2FtFAKXrEGZmrcXPlr5l6ThOQ9Q3gviF02UzWmBKnZQbwfTDOMo5ukHRej9K9hUbLtXqmYCoNozgXZPPzQQ8mOeLrGuZ95zUMHm%2F3MOCN9toKQkxIpozKpAH9ZUGB5gprE1eFxL1mZuQhCdk%2BXSCD%2FsSMQZItSM6LDsEXLLdGL%2BMaadorkwtVZbwQLOjIe7lXG0TAOm7EkboE6xQSI%2BQ3T2MRys0FD1qBrvo79oGgcHyqh6Aa%2B3XBCGNuXSBisW%2BeyInGUu9kKAYAopBXKI5TWMlZsgUOKH3UoPURIVDmpR53KTlFT1oE7M1iDuu%2FsWFFYOn45Hir3vCHgAqqUblJEe%2BHWLt%2BYeK81VKj4xpx9sEzkuhIk%2FKd8Z8GAsfPacu5fKj%2BJePCmFTU%2FoH8YDmzIwjJAC36e8r3DttY0xS6yvi2tj8J4blyQLVC%2Bt1pckz9jiz0GaRauQRuwrCTon7poHwXu4X9uyot6Ei1hcEbiYCGjS6kRvALv4dtqz97a5v5n7hZcOQU6I3aLyvqoUmyu6w361HoXdbP50hqEke0ZsAbtUweanwXb3qTrxBV0xkbpUi0sLWNuVaINBAU%2BF2i2PEsQivzdjpn2YduSOTCy%2F%2F3IBjqkAQo4fXuVnYfOHl1Pov7ZFQSAeXrDsz5Zy%2FklW45oF21NuSGSVUgpatR8Yh6Qt92XwylqfB5msTP3v5CZkDDfLXcDeoFl77eT4Y6JMwhew6HXtx39rW1vvGqnTDp8TN%2Fh5dmQ7IXt5%2FvYgo%2Fi%2BhIGpjaA%2BF3t3Bf%2B5lKQgNkGJc4HeEmkfdZxgQI17lo%2FAhnueH1b0CTqEtF90y%2F9US%2FcCvIBSRl5&X-Amz-Signature=2efc8e10f62d88f7c52c07f9ba2a0ce6f8ef6a7e8c8f181ed6f10bac041daa89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

