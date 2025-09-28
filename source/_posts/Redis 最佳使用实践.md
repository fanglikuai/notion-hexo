---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZW74EFQ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCNRnl100aDxHM1t9cnWMdFXaHqjYVlRQzvoOEhzdJRUQIhALcn%2F1zbz7PDXLEfrAdLXnu9X5%2F0l2Cory9QLzvh69%2F2KogECL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxqekgZj4EWuA0B2mgq3AMf0nMdD0qXm6fCDdMzb4ResG9xt7tS4G%2B9Rj7UHV4ykobRdQdFwrdFskk8JFZBTPrdxxQn5OCf29pV44mN%2FvWXMj0ST9EUwqqAtcxM%2Bj4bJ4%2FEAKgihpnP%2Fa4QU4x%2FROLnBDpW5z9TueAElwfirdAuZtxbJY%2F1IOpOzX18NVSbNLfyaqdCI7eMbVzNuLlqQ%2B4A7%2BFiwU25h80yf%2FA%2F%2BmRm93JRpO191Br31gtj376uFiAUdzzj5qwZQbL%2FMPVOC8CnAw1kkgwrkd7aqnfGimzYsnGdXnAYbQHGSOFhsDExLCFCNmtyE97QX6QIEdNR8kspwJ5%2By%2FNEuDqoLJekBh9c%2BNHpF1htHyqburbObxlakOztRn4v%2FNY2cJAchXObC0emUqa9ABmi5k4%2FVYarXA7en2cJucLu2I8YQqQlYkEmvtVX37zfDeZpfQ6dUt5g6pQCOq6H9k%2BTbv5k9We9wgPqb1ohnHISIpfysyleUnolK3CozU0Q%2FL2QIOB4%2BMyYb50aFpnj1lpi4dUfXn28Olbi%2FY5Tfc7%2FN%2Fb5SAzMac4kY2XpYNAB%2BAFShicQK9CD5wXelbm6MWcR3a%2BkiPHC6vHxDAjTxYo1a2hexI7A4wVKa0f1aiqyN%2FVOcFdCmjCZ7%2BTGBjqkAY9DqvluwQ2brKh47%2FwWqTJf6F9FDMw9Z58hzwHp%2Bs6VWjuXqS1ZBZmDtoYhQ7GBMU4%2BDC95SK7%2FvnW0Bi8a%2BpJRi61AfqqUIjHlXrrxIfPk2htLRyo2us8xlbHo7EE4fy6C39q87FXMfEUtMtm2%2FpaOLdtEI22O%2F9Kbfph%2FfhFbNa47eH0VmNav61TpjpAplsGLpIzhAzeecBdFWuSLsIJYlkD%2F&X-Amz-Signature=edf39a90a4e969ed1a39fc8a89915c88242d2beed4b1bbd02f2b7eb6c7b1a841&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

