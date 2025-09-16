---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NS4BNMB%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDizhGb5SnEUhJBSW%2BHrIVpYDNueJ7nER%2FHsTmkT%2F07hAIgdjBpU0vo61KmlyHvuJLxjXLjjtJSuXAA0ShmZCTfam4qiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDrdOI6oLQ%2Fr2w%2FKVSrcA7kRm3akmp5NwsF67MjNL%2BhU6eNUEp139Tq2nBAe378YXcbhzKTUjExGdDqy9Ir0Z%2FkCCB2jIBklSiQf2Ple9HNtP6EMLwU3ztdR8IEujD31A2Un1IxgDhpkoiIpf9K9C3lAqBiNqoIW7YTBy1D3pDdpZ%2FkqT6yAocU7b6TJDuWYzNCYPDHmcBoJZ7NUyNfQGTv4EMPHawerasnuD4DankhWurKl68TfFSd7UjnexV45LL1hhBE3C0Sr2hHx9Slfu3JobkIep8dinTV27ttnH5E%2B1BQFMcl8y1Srf1iyLxD%2BXJ5goeP1eCuX1LgMKl%2Boa5e4sn4NAGpWAnfxZUHh4OmZXp9wyM3gMccK%2FyHCZq%2F8JzeBYOqisEH2KTbmIUqSeo4HTVuHCLj0IT57uygD47%2FdEN4UBrx64sx%2BC%2FaSlhcKm%2BBR%2F1gSSSlDNPZDAfg2EkFiq12iXCiUhIeJD2Q41t1ePAJ4hNsGHCRL9JhScFS1V%2FiSko3Uaj4TZBrsyxLArHC8vtBTue4y4fG0vQfZQ5UDi16M%2FazFFDOHnc469p9cMW6c%2FyUzs%2Bbm9aDuZaK6pEkBP1mISXLbzaDtC8rWSpU5kecJcmpBmarKLLHP7Vtx2kGBxfC7m0w4fU%2BmMIzDpsYGOqUBsSonoYimvFQCUXS8ArkwvZGPpADwki%2B60Snv%2Ftujud5r%2BCRyJTskCk%2BZfGSsRq0lzhBs89QLzTrrjY%2B2c49C8Ix94b90KkSBSSh97RfaVmOEweZhFg9AojJqXZdTPEvayH%2By03ZALw7JAbu1RtTrCHm1D5hG7eypqRUo2luKw7Bbb%2F%2FmmSoISbHxgjSgTHJ1%2BXPJFH3T64k7i6m5p0Q38ztASD8B&X-Amz-Signature=0fbc50ab1731a9502438d318ac9b26ad14495294b2780f9fb7655f337b0a38d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

