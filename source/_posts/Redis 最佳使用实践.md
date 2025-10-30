---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z52N2COV%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIAKAUjAmPzbtgwsylJsPKzSwG%2BBEd6Sv3Mjnk1wheW0BAiEAyXQOrnwWWVd282PNfR7%2FUi3CdmBuvYe4ejvUn%2BXkCTQqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAysy5gjNIKpZW%2BRcyrcA5F5199uJn%2Fre8jy7Frsdb32SVBaiHgm4G3ADayfe2AP0YdzZmSMtBwEbLPYo16jgVCTDgpM4qtsjEtu8RVkdtOmuIOh%2Fu0bGRuYPDHwc4lsb0Bl52%2F4OnE43UTeEHT39XcfiAwtcAjnnoXMJyj7wEw6H6LamvM1Ox57uX5s8QwZlfDyktYzEOKVq54Zxjlzc%2BSgVoExSanR25cTE%2BNcHFhiw4RRHWvw70Rmfto1VtZ5XquBSj85z6jzTyR2ZPF9Qh6QvXSSN7bBwDNgFZoUv%2BbLa4nM190%2BmgBgZTVD35xAR%2BThomH0wwHPt01mTwtOarVRJoG1iAU6wKNrhkfBQd03frjqKiU1O6lFpQ2ynERCdsnsuzZJLEExndM8sypIue74c42FlSZEWLFftmtisSc9r9mLt0rJV7Hm7stCWFrdRc0k68YYOKDEfgiITezSkJXy7Y6EJja2ix3FhJvu2hG%2FctHjl84XeijiHHrk7UhOM0U%2BuH7HOuvPry1MNnuh323MKcDmwaHQ%2FYsAsszXUX2UogjMt1J7%2FfChJpZ%2BZ80TJNkJG0UQRenlg1y4KkOPorzB95lKGLH2ObLK5ZC%2FZ8soJ3aCJiBrmieKMecKduoieVGew6Mf%2BL%2FvbgR0MInRi8gGOqUBIUuZTl7g7MZ9JWURWq4dYNbyGr6ff4VqxSM9KIS0tNN3SUtoHuwfwX4fXayXK70RGzwY3hbs%2BjtBlrRDf2vEv4YtmSWnDXxh1ie8wcDUwynA1YNygRNyM5e0GVK%2BIVcw61Nb%2FlEm6Prbx0D3sytRj4Y6KoVaJkNnBrZQvItm%2BALJhN1OPoI2q15Zd6%2FiicPIHnqa94LiiGtt93Noj9IbdX2qbxSa&X-Amz-Signature=1ef30935f78609c425d867e8c89ba19bfba2bd9e46e04a807a41893d089df449&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

