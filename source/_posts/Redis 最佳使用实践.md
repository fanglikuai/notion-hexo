---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KF6JAR7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIHVvEMns%2BimOVd9Ov12nSmhK%2Bpdu1iEiR8lAPzlQoQcFAiEApIh5ruJSW%2BJ5ErF5sIOreGRsJdG7aPJu1h2JIP55tEIq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDFNqyuLbMdtRn4gVpSrcAwcmnraMoIbhZYwEQ6PrfSLItEUjEJA1o7%2FSbuss%2B26O0ZoBhEGsvvU5mm4CD%2FNrH0Q7iyXxLS4Oeyqd0qa4AC4kD46u%2FYDk%2FmhybyDx9eMBLhcnk8PWF%2B0%2BGoUCgFhrODNF4WsSi2Sd33%2BJlwOZwv7bneJTjJjqeJ6kkXoChquP0%2Bm%2BbBV2Gbeg9iAjgm8STdO76fs4NkjtMR7CtoRdcBMw5hoNzpITr28PU9%2Fyf5RaVBnmHOWdHXPN%2FurVfxEMzUb1KvKITlNdkbkaZ6XIuv6axsQgNxlfYNZo23WyNnmtqZrcqIZ%2FxQEtfCvw27lQ%2BbSeTPQEJL7HvGC6n6343WcWYn%2F8beRvTkLgV%2FC%2FCKnHOCLKS9jL7PYndBolErrJ%2FE6HWwuF6Am7L4AuQvMY7LW4EFWPEfLk1neJMePy3UWPDnryJ0MiZSphcK4InYAtpxH39nYjmxIz2uWwpRgXk%2Fvw3PZD4K24Uz5A43HBpRbNNMAch9szasDa0MDFKM99ZsMM1r%2FRuiisR9Fox4DHPzoYHHkaA958erpgIIIrvxrFx8txJvGkZEk0DcoFAaWNCib8zTigSEW7Be%2B1IVlmoNcJkOhLh1sJmzzyxAC5WmPJhuhtTYtGgxVjq7EnMKOilMgGOqUBohA0WEegpRFcSEJKp06%2B1z6Ps%2B%2BQGcDR%2Fe%2Bdpvji8AtzVUgpg%2BsPVM9z5WuR%2BxOu2nljBSsIw8Z%2BuJSSpb58xfvRK2Aqd1t2XyW2lz8OgElK19iWu5PhYSX09no9uaSe6EuozC1Lg8WF5ATGlSZyICZ9hTt%2BFeekfJmmQ3NEYEow%2FOr3h7%2BG9GDE4m9CBkiuH3QoTwGziWFLLWtFh8u9vHjWL2Ck&X-Amz-Signature=92298ac78e259dd9625bf6665ee1e4a1fe773c82426909a7804c8bdf0e45fbf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

