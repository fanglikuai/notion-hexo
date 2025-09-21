---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC5UAMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzmv99YQRukgJVJtJePeGARe1fv0v%2BpGZKt%2F0WoVKT6AiBrQAjZifJTDB1SgWVhl7LD3b2T2UwZ3hAt6BVE1u8uhCr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjmek9NcEb8CsWV19KtwD2cIptCmM1iATcu8Oq7A2VR7UKsiFADptrRnufCvkdMylbKLiLF4ChL0Hui8KgO77I%2BbuJX0edspxImbG6eL7HxsSF0dSaBe5xM3zn1KWTfZad%2Bv8bBXK6qIzNcCKowvDuFTrYTlzyZ5jVnPaaqB1bqX89NabvO37Ka%2F9KBhdUz2KRyyFitmPhT5kTf3ZJwpFXKlYo3K43yx4i%2B0uv52lAxMBQXivkM%2FetD3tE5vXvsUpsK8R5g4KYGPiFybvLGaWT0bQTtHP538KgED7BsuyMtByGyH8cB5tlIdsAd3HB9epnVA8nQ%2FtjbRQOhqfonJ%2BgVkFWMj2CRuSJ7W%2FsW3FIxVcwd7Zha7GsH8gwpxy7xj929Ge1MoLaX33Rejcehr3WXcRHYnEXFJnRALmDPnX3TvBkpterO%2Fu3BHvKs48mlRzf9LT%2BdItBas761L06BVq%2Bvbr7sqTjn4ZlfgbSNZpD4dub%2Blv8itVJnqH0ETG7yGC5FKus9DYykRYCt%2B0cyMdeTY7W2Vnr8vcXA%2BKKkxisQ2JYns%2BZ2iltbtfTUDdmJ2FwVnjMUg0wksQs1xCE286q2d7Fe6T%2F%2B9%2F8sVvJTJFe5ku2DRfq%2BJOZVEdpGqBIRzsAfjenxnqmuDBydAwnaS%2FxgY6pgGzcL8dCJ53NuDUT%2FAQTIBrBJpc%2BK992Bb2aecLzfLIRtHSq%2B9tWG1BxIFVuhvZF9fuCaVb8hXNAFfj8Ypsfs791z9VcyiDy2F2vHtuCvJYctH0sLGMzmdjDzHxAjXaAtf%2Fik%2F%2FNG97AczoYXniMhwm77fwvINVG1btYrkmk88c1eX8ZNynGJgjUDUnoZHK9teMQMh7AYzu%2BXBBByP40HZfqW6SiaHM&X-Amz-Signature=b387540d49e63cb0951bc7e757072241641c3606ae7bb67a33c7daa13162f8b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

