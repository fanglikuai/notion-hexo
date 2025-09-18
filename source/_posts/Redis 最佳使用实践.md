---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWDOTGXQ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCSbWYY7LC5vE%2FKtNTvNbOpP4qyRCPryRlEFDZehXmsAAIgUT5EEwIDAdBDkF4Yy8CVoj00h7R3pp6lgQMAciN%2BYgwqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAQk0EEI0O9sOPBNdyrcA0sgdO%2FNfANd95dAKpHPeNBnZx6C0Hob9N%2Fp%2FYexJxTo5MUhjBA%2BsmVduswCo9DWZFkHlkO1HRvvZs2OZIS6nfJt4q%2FOz6yTPDxi6pCvE6nhrOQEigl9YYv34YoIQQbg7J3Bw9NNG0%2BeopLH9byVdqKu4jSI%2Flg7xuL1YMvI59PsoMwTfhKmLrrp815M7Q6kH3Il5A9DD53rx4kytzzztTLwv%2B5etXoV9WS9t%2ByjXZFw6bX46Vzv%2B3LMX8lfxc0mouihyANPiVqAEXWNvQxpFwj7b%2Bh7Ufg92yhDZ%2FRmGP9%2FMTYrHCWo945VwGDhHL9uVr0Jeb7Gc%2F8Jt3bMZNaWVHGMiY2LXP7bpWBVZlTdhqf7hheDfg65TJUjgByYRAy93kiDNZGYIFJelo1BzLVbWF75bruOdegt%2Bwx4X81CRN8ieK65Clj3yoh6i%2BxUnsB0v9vZbYbxPkqqaHIAdlZUQH08SR%2FkGNpkDQ8BIvqc5srdm0lxjTkItwFFpsho6JP5lB1X3WmBe64SDHtVLEdpBOQIPxbuM5Nr6GsgCNCo0Dki%2FleCEOxIIFMPgG3Ccr8Xj1I6fODf4ulnbNen8bpN5SIW4vUJogCQslD2vaDRPigsTs95K9W7oBsFnozTMLi9rsYGOqUB8dVOSdhDzSrn9p87CsOoQ5WsQ5kQp10PsZ%2FTsi2Fq%2FNMyFphVmJfhv2rNwJ4RhxkjeQbChVAWLmzNYEE8aqLgW19T2whLnpWy2nW%2FFnbIzhIuRIIj33Bl4tGlH%2FBGdXzODy59H7bymeetCmeWHaYud%2FAPCmfGqWBzsZb4TKld6ZAJ10Y%2Bhz00KIGUtz0kVWj3p4NwcpNfoAESTYmLyTpK5GkpDh2&X-Amz-Signature=87d15ce2de83472c9a55df5740d44ef8a233c280a1152f8e2b545d3c35fa5ef9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

