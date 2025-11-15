---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662VGZU5ZT%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T120102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaSoKrDHp%2FKJW6cQVLPwrvdh%2FwtbgVUooqyH2r95j%2FegIgYGo%2FQkFVTIU%2Fb7WfwzasSNcdMYGmBKo%2Fb53hzcQs1gwq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDOdVqn7KsbyUj2txoircAw32S%2BoUBQwdZ8SJ3EbRbUrnsFdtpHXuHp%2FX6iakrsK%2B8VVZkdmoiB%2FLAAjEVPldHgCMOQC8bvEeURGUphRRAoHF%2FQwkPU1uVXs7vCV8DXOnLcf1ZSUKzJXZ3ZPl18RO%2FwuzwSqHlrbeM2vvSGPdwBelimRuNr3A91C%2FIVKY9LFF0jokqVdjQMxLo%2FHueoUJ3zrsp5bb7UfK%2BeGs5k9dtE2G2%2BFDOqxLtseNM3L6r11b%2FAeReKQ4DCKx5gYyHPNBwz8Ht6Ss76JFspr2er7UC1eH32q3X7iIycAGKvsT6Cp4uw6nkyESjHwSF1Gs5E%2Fq%2FvWmDAVyHvgNhTkIikWUqDYMm8tjdtpcubSqpRQgOMHSw2CDLgAUbcl%2F8imDDTBMMgIkDBa0Q%2B2L%2FFhepdC9CTSYUYy64ELEZ3D5nFXzIzsquTAtJxfnj%2BBfcWzqybnrDN1Fjb8Hqs%2BmkkvwIAzXBNFYf2gVwh5YU4f6e6dwunn%2Fuo6%2BBYvNlYfNM2L8sRHJIgObZEbhyy9%2FWRNIRsLpGTuszrKDx9EYMpfJaJOYAxSr3kYd8tVFR7zGRwarw2E78zabl27NKz7g32UZOaH%2FdalYQunuE2Bo2T1jpyhwzuozSt6oUGCgDL8VZVUXMPKD4cgGOqUBo07YkXd13fkxf8oB%2FGXqi3lPO64S%2F7Ul1UbmOYnFNJFUj7u%2F40Qq9aNHo5kiTeA8oDA9qLsLYYQJ1wnKnXoQWALOkcA7D6gbOs7gXSe2SaHRk70r0Ze3fwz61GRDI5y12kz543eqcVQLaqKR0QuKv%2Bb0O6DzUVoLQBUTBcDwfmpguHpVLhsR94gtrt9Avkp7GnYlsCGlsKpraLm8IDxKr%2BcfHXyd&X-Amz-Signature=3a4f34c5b0c8dfa8212d273959c56f8f01d3c8b3382d54733c7139043e0b5d29&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

